---
layout: default
title: Apache Cassandra
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/12nosql-apache-cassandra/
nav_order: 12
---

# Apache Cassandra

## General

* No JOINS
* Think query first
* Denormalisation is obligatory
* No tolerance for duplicate rows (if you write a row with the same primary key as another row it will overwrite the data in that row)

## Primary key

* Needs to be UNIQUE (see above)
* Is made of parititon key and clustering columns or only the partition key of there are no clustering columns

### Partition key

* Determines how the data is partitioned across the nodes
* Choice of key depends on the query that will run against the database: the partition key MUST be included in the query, i.e. queries must contain a `WHERE` clause! The reason for that is that data is spread across multiple nodes. By using the `WHERE` statement, we know from which node to get our data and serve it back.

### Clustering columns

* Used for making a primary key unique
* Clustering columns will sort the data in ascending order in the order of how they were added added to the primary key
* Clustering columns are not obligatory
* Clustering columns CAN be used in the query IN THE ORDER they appear in the primary key definition
