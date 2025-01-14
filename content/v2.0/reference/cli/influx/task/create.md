---
title: influx task create
description: The 'influx task create' command creates a new task in InfluxDB.
menu:
  v2_0_ref:
    name: influx task create
    parent: influx task
weight: 201
---

The `influx task create` command creates a new task in InfluxDB.

## Usage
```
influx task create [query literal or @/path/to/query.flux] [flags]
```

## Flags
| Flag           | Description                               | Input type  |
|:----           |:-----------                               |:----------: |
| `-h`, `--help` | Help for `create`                         |             |
| `--org`        | Organization name                         | string      |
| `--org-id`     | ID of the organization that owns the task | string      |

## Global flags
| Global flag     | Description                                                | Input type |
|:-----------     |:-----------                                                |:----------:|
| `--host`        | HTTP address of InfluxDB (default `http://localhost:9999`) | string     |
| `--local`       | Run commands against the local filesystem                  |            |
| `-t`, `--token` | API token to use in client calls                           | string     |
