---
title: "Querying BigQuery with Python"
description: "How to query a Bigquery dataset with python"
lead: "How to query a Bigquery dataset with python"
date: 2022-06-06
lastmod: 2022-06-06
draft: false
images: []
menu:
  docs:
    parent: "Section 3: Data Acquisition Methods"
weight: 310
toc: true
---
{{< alert icon="💻" context="info" text="<b>TL;DR</b> Working with datasets. You will need to have the proper credentials to be able to authenticate correctly." />}}

### Create client

``` python
from google.cloud import bigquery
import pandas as pd
import os

os.environ["GOOGLE_APPLICATION_CREDENTIALS"]="./your-project-credentials.json"

bqclient = bigquery.Client()
```

### Perform a query and download it in .csv format

``` python
# Query string
query_string = """
SELECT *
FROM `project_id.dataset_id.table_id`
LIMIT 50
"""

dataframe = (
    bqclient.query(query_string)
    .result()
    .to_dataframe(
        # Optionally, explicitly request to use the BigQuery Storage API. As of
        # google-cloud-bigquery version 1.26.0 and above, the BigQuery Storage
        # API is used by default.
        create_bqstorage_client=True,
    )
)

# download your dataframe in .csv format
dataframe.to_csv('data/dataset/test.csv', index = False, index_label = False)

# print df info
print(dataframe.info())
```


### Upload a table to BigQuery

``` python
# table name
table_id = 'project_id.dataset_id.table_id'

# Since string columns use the "object" dtype, pass in a (partial) schema
# to ensure the correct BigQuery data type.
job_config = bigquery.LoadJobConfig(schema=[
    bigquery.SchemaField("feature_1", "STRING"),
    bigquery.SchemaField("feature_2", "STRING"),
    bigquery.SchemaField("feature_n", "STRING"),
])

job = bqclient.load_table_from_dataframe(
    dataframe, table_id, job_config=job_config
)

# Wait for the load job to complete. (I omit this step)
job.result()
```


### Delete a table in Bigquery

``` python
# table id
table_id = 'project_id.dataset_id.table_id'

# If the table does not exist, delete_table raises
# google.api_core.exceptions.NotFound unless not_found_ok is True.
bqclient.delete_table(table_id, not_found_ok=True)  # Make an API request.
print("Deleted table '{}'.".format(table_id))
```

<b>Sources:</b> 
* [BigQuery Documentation](https://cloud.google.com/bigquery/docs/how-to#working-with-datasets)
* [BigQuery-Python API](https://googleapis.dev/python/bigquery/latest/index.html)
