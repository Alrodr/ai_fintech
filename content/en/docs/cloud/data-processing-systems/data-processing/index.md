---
title: "Data Processing: Fundamentals I"
description: "Data Processing. Concepts at a high level"
lead: "Data Processing. Concepts at a high level"
date: 2021-05-01
lastmod: 2021-05-01
draft: false
images: []
menu:
  docs:
    parent: "Section 2: Data Engineering"
weight: 100
toc: true
---

## Concepts at a high level

In this page, we are going to start slow by learning some of the high-level fundamentals of data processing. We will go over some core, big data concepts. You may have heard of some of these, but there's no harm in having a refresher, so let's get started. So first we will answer the most important question: 

### What is Big Data? 

When people talk about big data, they are usually referring to 1 of 2 things, large data sets or the technology that is used to handle large data sets. What we are really interested in conceptually is the large data sets. But large dataset is just another way of saying big data. So we need to rephrase the question and ask, what defines big data? And this is another area where there are dozens of definitions in the wild. But generally they agree on these 3 characteristics sometimes called the 3 Vs of big data.

1. The first one is volume, just the sheer scale of the data being handled by a system. You can pick any benchmark you like for this, but in the early days, this was considered a data set that could not be handled by a single server. 
2. The next V is velocity. The speed at which data is being processed. That doesn't just mean being stored, but analyzed, visualized, or used in some productive way. 
3. And finally variety, the diversity of data from different sources in various different formats and a varying quality. 

You can plot these characteristics on a 3D graph where we have volume, velocity, and variety, as the axis. Let's start with variety. This is all about the different types of data we can process from simple key value data, through to tabular databases, images, audio, video, or other unstructured data, and even streaming data from mobile or internet of things (IoT) devices. On our velocity axis, we start with processing data in an ad hoc way, but we can move up to batched or periodic processing of data through to near real-time and actual real-time systems. In terms of volume, our third axis, we go up simply through the various scales of data from megabytes to gigabytes, terabytes, petabytes, and even zettabytes, we could even go further. So if you think about your dataset as a 3D shape on this graph, how far out does it stretch on each axis? If it looks like a big shape, it's probably big data. Now we know what big data is. 

![The 3Vs of Big Data](data-processing.png "The 3Vs of Big Data")

We are going to have a quick overview of the concepts of data warehouses and data lakes. OLTP versus OLAP or online transactional processing versus online analytical processing. SQL versus NoSQL and batch versus streaming. First up we'll look at the concepts of data warehouses and data lakes. 

### Data warehouses and data lakes

These are both abstract ways of thinking about how data is stored and like the term big data, they are concepts with several different and evolving definitions. I am going to try to stick to characteristics where there's some consensus in the industry. 

* A data warehouse contains data that is structured and or processed. By which I mean, the data may already have been processed or transformed to be stored in its structured place. This means the data is ready to use. It doesn't require any further transformation and can be consumed easily. For example, for business intelligence purposes or reporting. The downside with the concept of a data warehouse though, is that it structures are rigid, they have to be. But that means it's going to be a non-trivial piece of work, if you ever need to change that structure. Data in warehouses may also not be the most up to date because it may have gone through a transformational process to get there.

* A newer concept than the data warehouse is the data lake. Here, all raw or unstructured data is stored. To help you imagine that our previous data warehouse actually stores bottled or filtered water ready for human consumption. The data lake meanwhile contains every single drop of water. Data in our lake isn't really ready to consume, but it is ready to analyze. It's the most up-to-date source of data, but the tools you need to use it, might be more complex. The data lake is naturally more flexible than the data warehouse, because there is no enforcement of structure on the data. Now, at this point, you are probably thinking, why do I care about warehouses and lakes? But again, these are just abstractions to think about when designing data systems at a very high level. 

### OLTP, OLAP and ETLs
Next up, let's look at OLTP versus OLAP. Again, these are high level concepts for identifying types of data processing systems. OLTP and OLAP are both online database systems, that's the O, L, and P in the acronym. The difference between the 2 types is really how they are used. 

* An OLTP system usually processes a high number of short transactions like select or insert statements in basic SQL terms. Queries should be fast and the system should maintain a high level of data integrity. As an example, a database that records a purchase transaction in SQL database is an OLTP system. 
* An OLAP system by contrast will typically run a lower volume of transactions, but they could be longer running queries. An OLAP system might also run these queries over aggregated historical data, which could have been sourced from other OLTP systems, even multiple ones. So following our previous example, this system could be used to produce a report on purchases and purchasing trends over time. 

Another way to remember the difference is that in general, OLTP systems modify a database, and OLAP systems query a database. Now, while we are here, let me drop another acronym on you. ETL, which stands for extract, transform, and load. ETL processes can take data from an OLTP system and move it into an OLAP system. 

### SQL and NoSQL

Our next concepts, which are thankfully less abstract are SQL and NoSQL. No doubt you will have heard of these before, but as a quick refresher, SQL or SQL databases contains structured tabular data, which means rows and columns stored in tables. Take for example, storing a customer entity in a SQL database. The information we store is entered into these columns, which must follow a strict schema. SQL databases, are relational databases, which means that entities and tables can contain relationships to other entities, enabling you to build relatively complex queries. Most of us will have had some experience with SQL databases, such as MySQL or Postgres. NoSQL databases by comparison, come in a few different flavors. 

NoSQL is basically a catch-all term for anything that is you guessed it, not SQL as opposed to a single alternative design. NoSQL databases include key value stores, JSON document stores, and a few other variants. The most common NoSQL systems you may have heard of are *MongoDB* or *Apache Cassandra*. We will take a closer look into SQL, NoSQL, and the differences between them in our next chapter. 


### Batch and streaming

And to finish on something easy, our last pair of concepts are just 2 ways to ingest data into a system, batch and streaming. Batch data is simply data that is gathered together, usually within a defined time window and loaded into a system, all in one go. You will usually choose batch, when there are large volumes of data to load. For example, when importing data from legacy systems. Streaming on the other hand is just as it sounds, the continuous collection of data into a system. Data is collected as it happens if you like every transaction or metric being sent immediately to the system. It can be harder to process streaming data because of the sheer volume of it. But this makes it particularly well suited to near real-time analytics. Now, just to confuse this slightly, it's also possible to slightly stagger the input of streaming data with time windows and micro batches. But we will learn more about those in the context of specific tools when we come to them.

As you can see, these are the concepts you will hear most discussed when people are designing data processing systems at a high level. In the next page, we will look at the fundamental theory for ingestion, storage, processing, and visualization.
