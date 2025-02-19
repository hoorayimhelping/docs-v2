---
title: influx user find
description: The 'influx user find' lists and searches for users in InfluxDB.
menu:
  v2_0_ref:
    name: influx user find
    parent: influx user
weight: 201
---

The `influx user find` command lists and searches for users in InfluxDB.

## Usage
```
influx user find [flags]
```

## Flags
| Flag           | Description     | Input type  |
|:----           |:-----------     |:----------: |
| `-h`, `--help` | Help for `find` |             |
| `-i`, `--id`   | The user ID     | string      |
| `-n`, `--name` | The user name   | string      |

## Global flags
| Global flag     | Description                                                | Input type |
|:-----------     |:-----------                                                |:----------:|
| `--host`        | HTTP address of InfluxDB (default `http://localhost:9999`) | string     |
| `--local`       | Run commands against the local filesystem                  |            |
| `-t`, `--token` | API token to use in client calls                           | string     |
