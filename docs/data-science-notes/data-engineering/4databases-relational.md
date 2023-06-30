---
layout: default
title: Relational databases
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/4databases-relational/
nav_order: 4
---

# Relational databases

## Introduction

### Definitions

* **Relational model**: Organizes data into 1 or more tables with rows and columns
* **Relational database**: database based on the relational model of data
* **RDBMS**: Relational databases management system, i.e. a system used to maintain relational databases
* **Schema**: A collection of tables
* **Tables**: A group of rows sharing the same labeled elements (or columns)
* **Attributes**: labeled element or attribute
* **Records**: Each row of a table is known as a record (or tuple)
* **Primary key**: An attribute that uniquely identifies a row in a table. If several attributes can play that role, they are called candidate keys. Out of all candidate keys, only one gets selected as primary key, remaining keys are known as alternate or secondary keys.
* **Surrogate key**: A surrogate key is any column or set of columns that can be declared as the primary key
* **Composite key**: A key that consists of more than one attribute to uniquely identify rows
* **Foreign key**: Foreign keys are the columns of a table that points to the primary key of another table. They act as a cross-reference between tables

### ACID properties

* **Atomicity**: Whole or nothing is processed
* **Consistency**: Previous state is kept if new state not consistent
* **Isolation**: When a query runs, it is locked and cannot be affected 
* **Durability**: Once the transaction is complete ("committed"), it is saved to the database and cannot be lost even in case of system failure  

### Pros and cons compared to noSQL databases

| Pros                                      | Cons                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| Good with relatively small amount of data | Schema is fixed (all rows use all columns, not saving disk space) |
| Good for analytics                        | Cannot store unstructured data                               |
| Can use SQL                               | Low availability                                             |

## OLAP vs OLTP

##### **Online Analytical Processing (OLAP)**

Databases optimized for these workloads allow for complex analytical and ad hoc queries, including aggregations. These type of databases are optimized for **reads** (SELECT queries).

##### Online Transactional Processing (OLTP)

Databases optimized for these workloads allow for **less complex queries in large volume**. The types of queries for these databases are read, **insert**, **update**, and **delete**.

## Normalisation

**Normalisation**: Reduce data redundancy and increase data integrity (have data only in one place and consider it the source of truth). When data needs to be updated, it is updated in only one place (or as few places as possible).

**Denormalisation**: The process of trying to improve the read performance of a database at the expense of losing some write performance by adding redundant copies of data. This is done by reducing the number of joins between tables as joins can be slow.

Normal forms:

* **First normal form** (1NF): 
  * Atomic values: No lists, sets, tuples, etc. are stored, only single values
  * Adding data without altering tables: Ideally, no new columns should be required
  * Logical grouping in tables: Keep stuff of different nature in different tables
  * Foreign keys to link tables together
* **Second normal form** (2NF):
  *  Every non-primary-key attribute is fully functionally dependent on the primary key.
* **Third normal form** (3NF):
  * No non-primary-key attribute is transitively dependent on the primary key. If wwe can get B with A and C with B then we can get C with A transitively. In this case, A & B need to be in one table and B & C in another one. E.g. Take a table with customer_id, city, country. By nature, cities can only below to one country at a time and if, for some reason, a city changed country, country would need to be updated several times, whereas, if country was in another table, it would need to be updated only once.
  