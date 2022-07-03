---
title: "The Data Model: High-level explanation"
description: "The 3 basic stages of data modeling."
lead: "The 3 basic stages of data modeling."
date: 2021-05-01
lastmod: 2021-05-01
draft: false
images: []
menu:
  docs:
    parent: "Section 2: Data Engineering"
weight: 120
toc: true
---

Being a data scientist or a data engineer means knowing how to manage data, how to get it where it needs to go, into the format it needs to be in, and so on. At a high level, you will need to consider sources and sinks. That means where the data is coming from and where it is going, what it is consuming it. Can the data be consumed as is, or it is some preparation work required to make sure the format is appropriate for the data sink?

The data itself is almost always going to fall into 1 of these 2 categories: structured and unstructured. You need to think about how you are going to process and store that data. 

For structured data, you also need to think about the data model and if it is appropriate to the requirements for that data. Data ingestion is often achieved with either a batch or streaming model. So, you need to think about how you will manage these approaches. How you are going to build your data pipeline or if you need to consider latency or time windowing issues, for example. As I said, these are all high-level concepts, but we will explore them in more detail throughout other sections.

You should be aware of the basic concepts of data modeling. **Structured data always requires a consistent model**. Even if your underlying database allows for dynamic or changing schemas, you should still work to a good data model. If you are bringing data into your system, that model may already be in place. It may also feel like the wrong model, but if it is out of your control, you need to know how to handle that, where you ingest the data. And so data may require some preparation or transformation as part of the data pipeline. To refresh your memory, there are 3 basic stages to data modeling. 

### Conceptual stage
First, there is the conceptual stage where we think about the **entities** in our data, what are they? And what are their relationships? Let's imagine we're building a database for a bank. Our entities might be customers and credits. 

### Logical stage
The logical stage of modeling our data allows us to define the structures of our entities, what attributes they need, and what data types those attributes will be. At this stage, we would do some normalization of our model to ensure we are not duplicating any data unnecessarily. Although, in our very simple example here, that is not an issue. For instance, each customer will have a bank account and a related telephone number. Our second entitie, credit, it will have an amount and a recovery data for the credit.

### Physical stage
Finally, there is the physical stage where we think about how to actually implement our model, with what database, and how that would affect our choice of keys and indexes. From our very basic example, here, we might choose a relational database. When choosing relational databases, we want to think about good relational schema design. As the name implies, there should be relationships between the different tables of data we are storing. If we have tables of customers and credits, we might also have a table of transactions, rows in this table would reference the IDs of rows in other tables, defining a relationship between them. Sequel queries can be used to benefit from these relationships and join tables together to present data. Keep this in mind when you create your schema. So, you can normalize your model and reduce waste. For example, you wouldn't store a customer's purchases in the customer table, or the transactions of a particular type of product in the credits table. This is exactly why you use a relational database with carefully chosen primary keys and indexes. With these fundamentals in mind, think about how your database is going to maintain accuracy and integrity. 

This is a little more complex, but you need to think about the design of your tables and data types. And if they are providing the right information to your application or other consumer of your data. You also need to consider how changes to your data will be handled to maintain data integrity. If you are choosing a non-relational data system often referred to as a noSQL system, you have a few different types to choose from. So you need to choose what best suits your application. This could be a simple key value store such as provided by Redis or a document store. Somewhere you can put a hierarchical collection of JSON documents. Alternatively, you could be dealing with time series data, more suited to a high volume columnar database. As I said, these are all lumped together under the slogan of noSQL, but they're all actually very different systems. Finally, think about your pipelines and how they are going to get your data from A to B. In this simple example, our data is sourced directly from an application and further data is found in a cloud storage bucket. An event in either of these systems should trigger ingestion. Cloud Pub/Sub is a great way to decouple the ingestion of data from how it iss later processed. 

The pipeline may need to do some transformation of the data for which we will use cloud Dataflow, and then we will accumulate all of our data in BigQuery. Finally, we can perform analytics and visualize the results with Data Studio.











