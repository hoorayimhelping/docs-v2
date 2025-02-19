---
title: influx task run
description: >
  The 'influx task run' command and its subcommand 'find' output information
  related to runs of a task.
menu:
  v2_0_ref:
    name: influx task run
    parent: influx task
weight: 201
---

The `influx task run` command and its subcommand `find` output information related to runs of a task.

## Usage
```
influx task run [flags]
influx task run [command]
```

## Subcommands
| Subcommand                                       | Description          |
|:----------                                       |:-----------          |
| [find](/v2.0/reference/cli/influx/task/run/find) | Find runs for a task |

## Flags
| Flag           | Description    |
|:----           |:-----------    |
| `-h`, `--help` | Help for `run` |

## Global flags
| Global flag     | Description                                                | Input type |
|:-----------     |:-----------                                                |:----------:|
| `--host`        | HTTP address of InfluxDB (default `http://localhost:9999`) | string     |
| `--local`       | Run commands against the local filesystem                  |            |
| `-t`, `--token` | API token to use in client calls                           | string     |
