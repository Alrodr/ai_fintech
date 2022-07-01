---
title: "Python client for Google BigQuery"
description: "Simple Python client for interacting with Google BigQuery"
lead: "Simple Python client for interacting with Google BigQuery"
date: 2021-05-01
lastmod: 2021-05-01
draft: false
images: []
menu:
  docs:
    parent: "Section 3: Managing Databases"
weight: 300
toc: true
---
{{< alert icon="💻" context="info" text="<b>TL;DR</b> This tutorial shows how to get started with the Cloud Client Libraries for the BigQuery API with Python, step by step." />}}
 

_**Note**: To create this tutorial I have based myself on the following [documentation](https://googleapis.dev/python/bigquery/latest/index.html), to which I have added comments or explanations that I find interesting in order to use it in an optimal way._

## :zap: Quick Start

In order to follow this tutorial, you first need to go through the following steps. Note that some prior knowledge of GCP are required.

1. [Select or create a Cloud Platform project](https://console.cloud.google.com/cloud-resource-manager)
2. [Enable billing for your project](https://cloud.google.com/billing/docs/how-to/modify-project#enable_billing_for_a_project)
3. [Enable the Google Cloud BigQuery API](https://cloud.google.com/bigquery)

### Setting up authentication

Before you start you need to have read/write credentials for your BigQuery project that you want to work with. You can find more information about this [here](https://cloud.google.com/bigquery/docs/reference/libraries#client-libraries-install-python). You must first set up authentication by creating a service account and setting an environment variable (or getting your credentials in JSON format). Simply follow the steps in the link above.

#### Environment Variables

Provide authentication credentials to your application code by setting the environment variable `GOOGLE_APPLICATION_CREDENTIALS`. This variable only applies to your current shell session, so if you open a new session, set the variable again.

__Linux or macOS:__
```bash
export GOOGLE_APPLICATION_CREDENTIALS="./gcp_auth.json"
```

__Windows:__ 
* For Powershell:
```bash
$env:GOOGLE_APPLICATION_CREDENTIALS=".\gcp_auth.json"
```
  
* For command prompt:
```bash
set GOOGLE_APPLICATION_CREDENTIALS=KEY_PATH=.\gcp_auth.json
```

## :computer: Installation

Install [Python Client for Google BigQuery.](https://googleapis.dev/python/bigquery/latest/index.html) More information about the installation can be found in the link above.

```bash 
pip install google-cloud-bigquery
pip install --upgrade pandas
pip install --upgrade google-cloud-bigquery[pandas,pyarrow]
```

## :seedling: Creating a Client

A project is the top-level container in the BigQuery API: it is tied closely to billing, and can provide default access control across all its datasets. If no project is passed to the client container, the library attempts to infer a project using the environment (including explicit environment variables, GAE, and GCE).

To override the project inferred from the environment, pass an explicit project to the Client constructor, or to either of the alternative classmethod factories.


### Example 1: Querying Bigquery from Python

```python
from google.cloud import bigquery

client = bigquery.Client()

# Perform a query.
QUERY = (
    'SELECT name FROM `bigquery-public-data.usa_names.usa_1910_2013` '
    'WHERE state = "TX" '
    'LIMIT 100')
query_job = client.query(QUERY)  # API request
rows = query_job.result()  # Waits for query to finish

for row in rows:
    print(row.name)
```

### Example 2: Listing Datasets

List datasets for a project with the `list_datasets()` method:

```python
from google.cloud import bigquery

# Construct a BigQuery client object.
client = bigquery.Client()

datasets = list(client.list_datasets())  # Make an API request.
project = client.project

if datasets:
    print("Datasets in project {}:".format(project))
    for dataset in datasets:
        print("\t{}".format(dataset.dataset_id))
else:
    print("{} project does not contain any datasets.".format(project))
```

List datasets by label for a project with the `list_datasets()` method:

```python
from google.cloud import bigquery

# Construct a BigQuery client object.
client = bigquery.Client()

label_filter = "labels.color:green"
datasets = list(client.list_datasets(filter=label_filter))  # Make an API request.

if datasets:
    print("Datasets filtered by {}:".format(label_filter))
    for dataset in datasets:
        print("\t{}.{}".format(dataset.project, dataset.dataset_id))
else:
    print("No datasets found with this filter.")
```


