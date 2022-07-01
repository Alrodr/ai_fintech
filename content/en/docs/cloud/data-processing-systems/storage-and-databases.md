---
title: "Data Processing: Data Storage"
description: "Storage and Databases in GCP"
lead: "Storage and Databases in GCP"
date: 2021-05-01
lastmod: 2021-05-01
draft: false
images: []
menu:
  docs:
    parent: "Section 2: Data Engineering"
weight: 115
toc: true
mermaid: true
---

We will take a look at choosing and using data storage options in GCP. There are many different storage and database options in GCP and as a data engineer, you are expected to be able to choose the right option based on a set of technical and nontechnical requirements. In this page, we will take a quick tour of some of the options available and why we might choose them over others. To help you choose the right option, we can construct a flow chart based on some simple questions. 

{{< mermaid class="bg-light text-center" >}}
graph TD
    A(Structured) -->|No| B{Cloud Storage}
    A(Structured) -->|Yes| C(Analytics)
    C --> |No| D(Relational)
    C --> |Yes| E(Low Latency)
    E --> |Yes| F{Cloud Bigtable}
    E --> |No| G{BigQuery}
    D --> |No| H(NoSQL)
    H --> |Yes| X{Cloud Firestore}
    D --> |Yeso| I(Scalability)
    I --> |Yes| J{Cloud Spanner}
    I --> |No| K{Cloud SQL}
    AX{Cloud Memorystore}
{{< /mermaid >}}

### Data structured or unstructured
The first question to ask is simply, is your data structured or unstructured? By unstructured data, we usually mean binary blobs or files, such as videos and images. If you need to store this sort of **unstructured data, it's a simple choice, Google Cloud Storage**. If our data is structured, it's likely to need some sort of database and there are plenty to choose from. 


### Is the data for analytics?
To start with, let's ask if the data is for analytics. Are you going to use this data to analyze trends or maybe build a machine learning model? If the answer is yes, then great, but we've got a couple of different options to consider next. Do you need low latency access to a database? Perhaps you have some time series data that needs to write thousands, even millions of rows a second. If that's the case, you may want to choose **Cloud Bigtable**. And if latency isn't an issue and **you just need a big data warehouse, BigQuery is probably the right choice**. 

Now there are some other major differences besides latency here. Bigtable and BigQuery are both data powerhouses in GCP, so much so that they will get their own section in this web each, but just remember where they are in this decision tree and it will all make sense the more you learn about them. 

### Is it relational data?
Okay, so say your data is not for analytics. Is it relational data? The sort of thing you would normally see in a MySQL or Postgres database? Well GCP has some great choices here, but how much scale do you need? Does your application serve multiple global regions? Does your database need to be accessed with strong consistency from anywhere in the world? If that's the case, you should take a look at **Cloud Spanner, a groundbreaking managed service that promises high availability, consistency, and horizontal scaling**. 

If you just need a traditional SQL database, there are fully managed MySQL and Postgres options in Cloud SQL. **If your data is not relational**, maybe you need mobile SDKs or maybe you need some sort of NoSQL database. In this GCP decision tree, both choices lead you to **Cloud Firestore**. This is Google's proprietary NoSQL documents store with fully managed auto-scaling and offline data access. **It is the new version of Cloud Datastore**, which has merged with its counterpart from Firebase, Google's mobile developer platform. Now there's just one more product to add. 

This layout comes from Google's own recommendations of how to choose a data product, but they leave out **Cloud Memorystore. It is simply a managed Redis service**. So if your application needs a Redis key value in memory database, there's a managed service just for you. 

## By Service

### Cloud Storage. 
As we stated at the top of our flow chart, **it's ideal for unstructured object storage**. 

* It basically provides you with buckets where you can store files. Buckets can exist in a single region, two regions, or several regions. All options are durable, but serving from multiple regions can reduce latency. 
* Buckets and objects and side buckets can also be classified as standard, nearline, or coldline. These are indications of how often the data is likely to be accessed and allow you to pay less for data that is infrequently accessed. 
* One key feature of Cloud Storage when handling data is the system of storage event triggers. When an object is written to a Cloud Storage bucket, you can trigger a notification, which can in turn, cause some other action to happen. This is achieved using Google Pub/Sub, which we will discuss later. **Triggers from Cloud Storage events are usually the first steps in building a data ingestion pipeline**. 

### Cloud Bigtable
This is Google's petabyte-scale wide column **NoSQL** database designed for high-throughput and scalability. 
* When people think of NoSQL, they often think of document stores like MongoDB, but Bigtable is designed for wide column, key value data. Entries have a single key and multiple columns and the whole system is designed for very high volume writes, making it ideally suited to time-series, transactional, or internet of things data. 

### BigQuery
BigQuery is Google's other petabyte-scale data platform. BigQuery is designed to be your big data warehouse, storing incredible amounts of data, but in a familiar relational way. This enables data analysts to query these enormous datasets with simple SQL statements. BigQuery can retain historical data for very little cost, the same as Cloud Storage itself, so you can use it for analytics to form the foundations of business intelligence systems or train machine learning models on its datasets. There are also multiple public datasets available, which can be very useful. 

### Cloud Spanner and Cloud SQL
Putting analytics to one side for a moment, let's look at standard relational databases, although there's nothing standard about Cloud Spanner. This is Google's global SQL-based relational database. It's a proprietary product that provides horizontal scalability and high availability and strong consistency. "How is this possible?" You may ask. "Doesn't this break the CAP theorem?" Well, sort of, we will get into this amazing database later, and don't worry if you've never heard of the CAP theorem, we cover that as well. It's not cheap to run, but if your business needs all 3 of these things, Spanner could be the answer. It's in particular traction in the financial sector where transactions absolutely rely on such features. 

If you don't need all the bells and whistles of Cloud Spanner, or you need to be interoperable with open source databases, look no further than Cloud SQL. This provides you with managed instances of MySQL and Postgres, which removes the requirement for you to provision and configure your own virtual machines. Cloud SQL even has built-in backups, replicas, and failover. It doesn't scale horizontally but can scale vertically, which means you can change the size of the instance type if you need to add more CPU or RAM. Cloud SQL is now also offering Microsoft SQL Server in beta. So that's plenty of SQL options, 

### Cloud Firestore
What about NoSQL? Cloud Firestore is Google's fully-managed NoSQL document database, designed for large collections of small JSON documents. In this way, you can think of it a bit like MongoDB. Firestore is the new version of Cloud Datastore, which has been around for a long time and was actually created originally as part of App Engine. But Firestore inherits a bunch of features from Firebase, Google's mobile development platform. So Firestore also provides a realtime database with offline caching and mobile SDKs out of the box. Unlike Datastore before it, Firestore now also offers strong consistency. 


### Cloud Memorystore
And finally, let's take a look at Cloud Memorystore, which very simply is a service that provides managed Redis instances. Again, the benefit here is that you don't need to provision and configure your own virtual machines. Redis is commonly used as an in-memory database, message cache, or broker. Memorystore adds built-in high availability, depending on the service tier you choose and can also be vertically scaled by dynamically increasing the amount of RAM available to your instances. 