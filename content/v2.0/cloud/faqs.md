---
title: Frequently asked questions
description: >
  Sign up for and get started with InfluxDB Cloud 2.0 Beta.
weight: 3
menu:
  v2_0_cloud:
    name: FAQs for InfluxCloud
---
Q: How should the 72 hour retention be understood for influx cloud 2? Does it mean all the data I push is deleted after 72 hours or is it the time window in which data can be pushed, i.e. any data older than 72 hours cannot even be pushed?
A: Data retention is determined by the time at which data is written to InfluxDB; not the timestamp of the data point. You can write data with timestamps older than 72 hours, but 72 hours after that data is written, it is evicted.
