---
title: influx auth delete
description: The 'influx auth delete' command deletes an authorization in InfluxDB.
menu:
  v2_0_ref:
    name: influx auth delete
    parent: influx auth
weight: 201
---

The `influx auth delete` command deletes an authorization in InfluxDB.

## Usage
```
influx auth delete [flags]
```

## Flags
| Flag           | Description                         | Input type  |
|:----           |:-----------                         |:----------: |
| `-h`, `--help` | Help for the `delete` command       |             |
| `-i`, `--id`   | The authorization ID **(Required)** | string      |

## Global flags
| Global flag     | Description                                                | Input type |
|:-----------     |:-----------                                                |:----------:|
| `--host`        | HTTP address of InfluxDB (default `http://localhost:9999`) | string     |
| `--local`       | Run commands against the local filesystem                  |            |
| `-t`, `--token` | API token to use in client calls                           | string     |
