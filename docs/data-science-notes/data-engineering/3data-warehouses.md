---
layout: default
title: Data warehouses
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/3data-warehouses/
nav_order: 3
---

# Data warehouses

## Definition

A large store of data accumulated from a wide range of sources within a company and used to guide management decisions.

## Difference with relational databases

| Parameter         | Database                                                     | Data Warehouse                                               |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Purpose           | Is designed to record                                        | Is designed to analyze                                       |
| Processing Method | The database uses the Online Transactional Processing (OLTP) | Data warehouse uses Online Analytical Processing (OLAP)      |
| Usage             | The database helps to perform fundamental operations for your business | Data warehouse allows you to analyze your business           |
| Tables and Joins  | Tables and joins of a database are complex as they are normalized | Table and joins are simple in a data warehouse because they are  denormalized |
| Orientation       | Is an application-oriented collection of data                | It is a subject-oriented collection of data                  |
| Storage limit     | Generally limited to a single application                    | Stores data from any number of applications                  |
| Availability      | Data is available real-time                                  | Data is refreshed from source systems as and when needed     |
| Design            | ER modeling techniques are used for designing                | Data modeling techniques are used for designing              |
| Data Type         | Data stored in the Database is up to date                    | Current and Historical Data is stored in Data Warehouse      |
| Storage of data   | Row-store                                                    | Column-store                                                 |
| Data Summary      | Detailed Data is stored in a database                        | It stores highly summarized data                             |
