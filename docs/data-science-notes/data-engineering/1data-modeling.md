---
layout: default
title: Data modeling
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/1data-modeling/
nav_order: 1
---

# Data modeling

## General

- **Data model**: an abstraction that organises elements of data and how they will relate to each other
- **Conceptual data modeling**: Understand what data there is and how they relate to each other
- **Logical data modeling**: Map the conceptual model to a logical model made of tables, columns, etc. 
- **Physical data modeling**: Translate the logical model into DDL (database definition language)

## Kimball's bus architecture

* Back room ("kitchen"): Space that is not exposed the users, where ETL happens

* Front room ("dining room"): Presentation area, where client tools (e.g. SQL client, BI tools) are connected
  * Data is organised by business processes
  * Data is atomic: it cannot be divided further, aggregations can happen but ideally it ishould be possible to arrive at the lowest level as present in the data source
  * Uses conformed dimensions: Common dimensions should be designed in a way that they can be used by several fact tables
  * No data marts (due to conformed dimensions)

* The data warehouse is designed using **dimensional modeling** techniques:
  * This approach focuses on identifying the key business processes within a business and modelling and implementing these first before adding additional business processes (**bottom-up**)
  * Dimensional modeling always uses the concepts of facts (measures), and dimensions (context):
    * Facts: Typically (but not always) numeric values that can be aggregated as well as foreign keys to the dimension tables
    * Dimensions: Context surrounding the facts, conformed whenever possible (see above). They are joined to the fact table via a foreign key. Dimension tables are denormalised tables. The columns of a dimension table are called attributes

## Inmon's corporate information factory

* Client tools can have access to prepared data marts as well as the enterprise data warehouse
* Data marts do not need to contain atolic data (can be aggregated) as the original atomic data can still be access from within the same DWH
* The Inmon approach is **top-down**, as opposed to Kimball's

 ![inmon](../../../../../../assets/images/inmon.png)

## Wide tables (or OBT for one big table)

https://www.hightouch.io/blog/data-warehouse-modelling-part-2/
