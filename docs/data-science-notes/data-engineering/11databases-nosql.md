---
layout: default
title: NoSQL databases
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/11databases-nosql/
nav_order: 11
---

# NoSQL databases

NoSQL:not only SQL

## Definition

## Common types

* Apache Cassandra (partition row store)
* MongoDB (document store)
* DynamoDB (key-value store)
* Apache HBase (wide column store, columns can store more than one datatype, i.e. schema is flexible)
* Neo4j (graph database)

## Pros and cons

| Pros                               | Cons                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| Handles well large amounts of data | Not for analytics                                            |
| Can scale horizontally             | No joins                                                     |
| Built for fast reads               | Not for flexible querying (you need to know in advance how you will query your data) |
| Flexible schema                    |                                                              |



## Apache Cassandra

Keyspace: collection of tables

Table: group of partitions

Partition: collection of rows, fundamental unit of access

Row: a single item
