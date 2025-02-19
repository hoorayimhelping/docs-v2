---
title: influx org delete
description: The 'influx org delete' command deletes an organization in InfluxDB.
menu:
  v2_0_ref:
    name: influx org delete
    parent: influx org
weight: 201
---

The `influx org delete` command deletes an organization in InfluxDB.

## Usage
```
influx org delete [flags]
```

## Flags
| Flag           | Description                        | Input type  |
|:----           |:-----------                        |:----------: |
| `-h`, `--help` | Help for `delete`                  |             |
| `-i`, `--id`   | The organization ID **(Required)** | string      |

## Global flags
| Global flag     | Description                                                | Input type |
|:-----------     |:-----------                                                |:----------:|
| `--host`        | HTTP address of InfluxDB (default `http://localhost:9999`) | string     |
| `--local`       | Run commands against the local filesystem                  |            |
| `-t`, `--token` | API token to use in client calls                           | string     |
