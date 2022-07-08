---
title: "The fastest way to get a database in the cloud"
description: "How to make queries with Python—SQLAlchemy to a bit.io database"
lead: "How to make queries with Python—SQLAlchemy to a bit.io database"
date: 2022-07-07
lastmod: 2022-07-07
draft: false
images: []
menu:
  docs:
    parent: "Section 3: Data Acquisition Methods"
weight: 330
toc: true
---

{{< alert icon="💻" context="info" text="<b>TL;DR</b> Bit.io is the fastest and easiest way to set up a PostgreSQL database. You can load data to a PostgreSQL database by dragging and dropping files, entering a data file's URL, sending data from R or Python applications or analyses, or using just about any other Postgres or HTTP client. You can then work with the data via the in-browser SQL editor or any of your favorite data analysis tools: SQL clients, R, Python, Jupyter notebooks, the command line, and so on." />}}

[bit.io](bit.io) offers a full-featured PostgreSQL database that can be used in seconds with practically no configuration required and integrates with an ever-growing number of popular data tools. Any tool that works with Postgres will work with bit.io.

## Requirements
You will need: 

1. Create an account at [bit.io](bit.io)
2. Create a database: Click the "Create Database" button that appears after you first log in. From here, you can name your database and choose your CPU and Memory configuration. Each account gets one free database with 1 CPU and 1 GB Memory. You can also choose to provision more storage above the included 1GB if needed.
3. Upload your first data file

## Query the data with Python and SQLAlchemy

First install in your environment `pandas`, `psycopg2` and `sqlalchemy`

```bash
!pip install pandas psycopg2 sqlalchemy
```

Finally, copy the connection string that you will find in the _connect_ tab,  and run the following code snippet.

```bash
import csv
from io import StringIO
import time

import pandas as pd
from sqlalchemy import create_engine

# Define your PostgreSQL connection string here
PG_STRING = 'postgresql://XXXXXXXXXXX:XXXXXX@db.bit.io/XXXXXX/XXXXXX'

# Create SQLAlchemy engine to manage our database connections
# Note that we bump the statement_timeout to 60 seconds
# pool_pre_ping=True 
engine = create_engine(PG_STRING, pool_pre_ping=True)
# SQL for querying an entire table
sql = f'''
    SELECT *
    FROM "your_database";
'''
# Return SQL query as a pandas dataframe
with engine.connect() as conn:
    # Set 1 minute statement timeout (units are milliseconds)
    conn.execute("SET statement_timeout = 60000;")
    df = pd.read_sql(sql, conn)
df.head()
```

That's all folks!