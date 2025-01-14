---
title: Cancel your InfluxDB Cloud subscription
description: >
  Cancel your InfluxDB Cloud 2.0 account at any time by stopping all read and write
  requests, backing up data, and contacting InfluxData Support.
weight: 104
menu:
  v2_0_cloud:
    parent: Account management
    name: Cancel InfluxDB Cloud
---

To cancel your {{< cloud-name >}} subscription, complete the following steps:

1. [Stop reading and writing data](#stop-reading-and-writing-data).
2. [Export data and other artifacts](#export-data-and-other-artifacts).
3. [Cancel service](#cancel-service).

### Stop reading and writing data

To stop being charged for {{< cloud-name "short" >}}, pause all writes and queries.

### Export data and other artifacts

To export data and artifacts, follow the steps below.

{{% note %}}
Exported data and artifacts can be used in an InfluxDB OSS instance.
{{% /note %}}

#### Export tasks

For details, see [Export a task](/v2.0/process-data/manage-tasks/export-task/).

#### Export dashboards

For details, see [Export a dashboard](/v2.0/visualize-data/dashboards/export-dashboard/).

#### Telegraf configurations

**To save a Telegraf configuration:**

1. Click in the **Settings** icon in the navigation bar.

    {{< nav-icon "settings" >}}

2. Select the **Telegraf** tab. A list of existing Telegraf configurations appears.
3. Click on the name of a Telegraf configuration.
4. Click **Download Config** to save.

#### Data backups

To request a backup of data in your {{< cloud-name "short" >}} instance, contact [InfluxData Support](mailto:support@influxdata.com).

### Cancel service

1. Hover over the Usage icon in the left navigation bar and select Billing.

    {{< nav-icon "usage" >}}

2. Click **Cancel Service**.
3. Select **I understand and agree to these conditions**, and then click **I understand, Cancel Service.**
4. Click **Confirm and Cancel Service**. Your payment method is charged your final balance immediately upon cancellation of service.
