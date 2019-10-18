---
title: InfluxDB storage engine
description: >
  Concepts related to InfluxDB storage engine.
weight: 7
menu:
  v2_0_ref:
    name: Storage engine
v2.0/tags: [storage engine, internals, platform]
---

InfluxDB 2.0 platform

The InfluxDB 2.0 platform includes:

- query engine
- internal API 
- storage engine

The query engine sends requests through the internal API to the storage engine. 

The storage engine ensures the following three things:

- Data is safely written to disk
- Queried data is returned complete and correct 
- Data is accurate (first) and performant (second)

We built a time series storage engine one layer at a time:

- Write endpoint: HTTP POST /write in line protocol
- Read endpoint: HTTP GET /read line protocol from InfluxDB

<get into v1 and v2 differences in separate section? ) 

V1 Write endpoint specification 

HTTP POST /write
measurement,tag_key=value field_key=value unix_time
h2o_feet,location=santa_monica water_level=2.064 1439856000
h2o_feet,location=coyote_creek water_level=8.120 1439856000
h2o_feet,location=santa_monica water_level=2.116 1439856360
h2o_feet,location=coyote_creek water_level=8.005 1439856360

Implementation of the write endpoint specification/write line protocol to memory

```bash
# specify the database is an in-memory list 
global database [] string

# add the post body to the in-memory list 
func write(post_body):
	     for line in post_body:
		database.append(line)
```


Read endpoint specification 
HTTP GET  /read 
            
#read from measurement h2o_feet
?m=h2o_feet                         <- measurement

#read from field key water_level
&f=water_level                      <- field key

#bound on time, so we have a start time but no end time, so unbounded on the end time &start=1439800000              <- unix time
&end=                                   <- unix time (unbounded)

#get tags and keys from the location santa_monica 
&location=santa_monica      <- tag key and value

measurement,tag_key=value field_key=value unix_time
---------------------------------------------------------------------------------------
h2o_feet,location=santa_monica water_level=2.064 1439856000
h2o_feet,location=santa_monica water_level=2.116 1439856360


V1 Implementation of the /read endpoint specification/filter by string match

Global database []string

func read(request):
	read_result =[]
	
	for line in database:
	    If line.matches(request):
		read_result.append(line)

	return read_result


Durability: Written data does not disappear when storage engine restarts.

Implemented durability with the Write-Ahead Log (WAL)

On the write side, WAL is a data structure and algorithm that is super simple and powerful.

When a client sends a /write request, the following occurs:

1. Write request is appended to the end of the WAL file.
2. fsync() the data to the file.
3. Update the in-memory database.
4. Return success to caller.

fsync() is a system call, so it has a kernel context switch which costs something. fsync() takes the file and pushes pending writes all the way through any buffers and caches to the actual spindles or chips. fsync() is expensive, but guarantees your data is safe on disk.
**Important** to batch your points (send in ~2000 points at a time), to fsync() less frequently. 

On read side:
When storage engine restarts, open WAL file and read it back into the in-memory database.
Answer requests to the /read endpoint.

In 2.0, we modified the /write endpoint by adding a WAL file. 

Implementation of the 2.0 write endpoint specification/write line protocol to memory

# specify the database is an in-memory list 
global database [] string

# specify the WAL file
global wal file

# add the post body to the in-memory list and WAL file
func write(post_body):		wal.append(line)
	     for line in post_body:

		database.append(line)
	     wal.fsync()
func startup():
    for line in wal:
	database. append(line)


1 WAL provides durability.
2 TSM is our data format. TSM handles more data/store more data. If storage engine terminates bc of OOM memory probs, then WAL is read back into memory, crashes again. Also, queries get slower as data grows. TSM provides efficient storage and query of time series data.

In TSM files, we group data (field values) by series key(m,tags,and field key), and then order field values by time.
Point= series key, field value, timestamp

Series key == measurement, tag(s), field key
H2o_feet,
location=santa_monica 
water_level=2.064 
level_description=”below 3 feet” 
1439856000
H2o_feet,
location=coyote_creek 
water_level=8.120 
 level_description=”between 6 and 9 feet” 
1439856000

4 series keys/cardinality 4:
h2o_feet,location=santa_monica level_description
h2o_feet,location=santa_monica water_level
h2o_feet,location=coyote_creek water_level
h2o_feet,location=coyote_creek level_description

One series = identified by series key; has field values and timestamps; ordered by timestamp
h2o_feet,location=santa_monica water_level

1439856000 2.064
1439856360 2.116
1439856720 2.028
1439857080 2.126
1439857440 2.041
1439857800 2.051

If you write code to handle this series (if you were to implement TSM), notice typical patterns in time series data:
- regular intervals (in this case 60 seconds) 
- minimal variation 

This gives us an opportunity to not only store by column format, but also compact data nicely. ***To compact data, it’s efficient to store data on disk as store initial timestamp, the interval, the initial field value, and then only store (w 16 or 8 bits) differences between field values. Or same field values as one.

TSM
Data is organized in column-oriented storage. Because data (series) are grouped by series key in TSM, a query can quickly find data for a specified series (fast queries) without searching the entire dataset.
**Note, in-memory database (list of strings) is still valid. 

Write data with TSM 
Get a write request from a user, InfluxDB writes request to the WAL and store the request to in-memory cache. After collecting ~10 minutes (or 25MB of data), InfluxDB takes a snapshot of the in-memory cache.
Puts snapshot into a TSM file.
Truncate the WAL (how much?).
Clear the data from the cache.
 So that's (what does that refer to?) configurable in the InfluxDB configuration files.

TSM lets you add scale to store data beyond:
the size of memory on the system
the size of the disk with compaction 
Get data from series you need, rather than iterating over the entire dataset.

FEATUREs
V2 durability
V2 store more data. But, If storage engine crashes due to OOM issues, and starts back up, WAL is read back into memory. Also, queries get slower when data grows. To resolve this, we introduced TSM data format (group field values by series key and order by time.)
Faster queries; we solved the problem of queries getting slower as data grows, but now queries are getting slower as cardinality grows bc queries need to iterate over all series keys. So TSI index comes in. TSI stores series keys grouped by measurement, tag, and field. Given a specified measurement, tags, and fields, what series keys exist? With fast series key lookup, cardinality capacity is much higher.
TSM is how we store data; TSI is the index used to reference the data.
SCALE (Enterprise customer wants to scale further) In bigger datasets, most queries are on recent datasets, can we optimize this somehow? To do this, we want to:
Partition the read and write load over time
Distribute load and capacity and over multiple storage engines
	We do this with shards (stack, package, bundle), which is a directory with WAL, TSM, and TSI files. Shards are time-bounded. When a shard duration is older than RP, drop shard.
Horizontal scale:
	By hosting shards on more than one storage engine:
		Storage capacity scales
		Query load capacity scales
		Replication is possible
 

