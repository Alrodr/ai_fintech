---
title: "Database snapshots with BigQuery"
description: "How to create database snapshots with Bigquery"
lead: "How to create database snapshots with Bigquery"
date: 2022-06-20
lastmod: 2022-06-20
draft: false
images: []
menu:
  docs:
    parent: "Section 3: Data Acquisition Methods"
weight: 320
toc: true
---

{{< alert icon="💻" context="info" text="<b>TL;DR</b> In Finance, some machine learning models need to be trained on a daily basis to have the best possible performance (data are constantly changing). When it comes to backtesting, it is useful to have databases of previous training to reproduce the results. I show you how to deal with it a Scheduled query in a super simple way." />}}

### Write a multi-statement scheduled query

1. Permissions and roles: `BigQuery Data Editor`
2. In Cloud console, go to the BigQuery page
3. In the Editor pane, enter the following `CREATE SNAPSHOT TABLE` query:

```sql
DECLARE snapshot_name STRING;
DECLARE expiration TIMESTAMP;
DECLARE query STRING;
SET expiration = DATE_ADD(current_timestamp(), INTERVAL 40 DAY);
SET snapshot_name = CONCAT("BACKUP.TABLE_",
 FORMAT_DATETIME("%Y%m%d", current_date()));
SET query = CONCAT("CREATE SNAPSHOT TABLE ", snapshot_name,
   " CLONE PROJECT.DATASET.TABLE OPTIONS(expiration_timestamp=TIMESTAMP '",
   expiration,"');");
EXECUTE IMMEDIATE query;
```

Where:
- `PROJECT`: Name of the GCP project that contains the database
- `BACKUP`: Name of the dataset where to save snapshots
- `DATASET`: Name of the dataset where the database to be snapshotted is located
- `TABLE`: Name of the table to be snapshotted

4. Click Run to test the query
5. Create a new scheduled query