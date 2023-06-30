---
layout: default
title: Dimensional data modeling
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/dimensional-data-modeling/
nav_order: 3
---

# Dimensional data modeling

Dimensional data modeling is a technique used to design data warehouses that are optimized for querying and reporting. It involves organizing data into fact tables and dimension tables that are connected by keys. Fact tables contain numerical measures and foreign keys to dimension tables, while dimension tables contain descriptive information about the dimensions being analyzed.

## Fact Tables

Fact tables are the centerpiece of a dimensional data model. They contain quantitative data, or measures, about a specific event, such as sales transactions or customer orders. Fact tables also contain foreign keys that link them to related dimension tables. Common measures in a fact table include sales revenue, units sold, and quantity shipped.

## Dimension Tables

Dimension tables provide descriptive information about the dimensions being analyzed in the fact table. For example, a sales fact table may have dimension tables for products, customers, and time periods. Dimension tables contain attributes that describe the dimension being analyzed, such as product name, customer name, or date.

### Degenerate Dimension

A degenerate dimension is a dimension that has no attributes other than its primary key, and it exists solely to group and filter data in the fact table. An example of a degenerate dimension is an order number, which is used to group and filter data in a fact table that contains order details.

## Schema Types

There are three types of schema used in dimensional data modeling: star schema, snowflake schema, and galaxy schema.

### Star Schema

The star schema is the simplest and most common schema used in dimensional data modeling. It has a single fact table connected to multiple dimension tables. All dimension tables are directly connected to the fact table, forming a star-like shape.

### Snowflake Schema

The snowflake schema is a variation of the star schema, in which some dimension tables are normalized into sub-dimension tables. This results in a more complex schema than the star schema, but can save storage space by reducing the amount of redundant data.

### Galaxy Schema

The galaxy schema is a more complex schema that includes multiple fact tables that share dimension tables. This schema is useful for modeling complex business processes that involve multiple measures and dimensions.

## Slowly Changing Dimensions (SCDs)

Slowly Changing Dimensions (SCDs) are a technique used to handle changes to dimension data over time. There are three types of SCDs:

### Type 1

This technique involves overwriting the old data with the new data. In other words, the old dimension record is simply replaced by the new dimension record. This approach is appropriate when the historical values of the dimension are not important, and the latest value is the only value that matters. However, this technique does not maintain historical data.

#### Example

Before:

| customer_id | customer_name | customer_address | customer_city | customer_state |
| ----------- | ------------- | ---------------- | ------------- | -------------- |
| 123         | John Smith    | 123 Main St      | Anytown       | CA             |

After:

| customer_id | customer_name | customer_address | customer_city | customer_state |
| ----------- | ------------- | ---------------- | ------------- | -------------- |
| 123         | Jane Doe      | 456 Elm St       | Anytown       | CA             |

### Type 2

This technique involves adding a new record to the dimension table for each change that occurs. The new record has a new primary key, and the existing records are not modified. This approach maintains a complete history of the dimension, allowing analysts to track changes over time. However, it can result in a large number of records in the dimension table, making it difficult to manage.

#### Example

Before:

| customer_key | customer_id | customer_name | customer_address | customer_city | customer_state | effective_date | expiry_date | is_current |
| ------------ | ----------- | ------------- | ---------------- | ------------- | -------------- | -------------- | ----------- | ---------- |
| 1            | 123         | John Smith    | 123 Main St      | Anytown       | CA             | 01/01/2021     | 12/31/9999  | 1          |

After:

| customer_key | customer_id | customer_name | customer_address | customer_city | customer_state | effective_date | expiry_date | is_current |
| ------------ | ----------- | ------------- | ---------------- | ------------- | -------------- | -------------- | ----------- | ---------- |
| 1            | 123         | John Smith    | 123 Main St      | Anytown       | CA             | 01/01/2021     | 04/03/2023  | 0          |
| 2            | 123         | Jane Doe      | 456 Elm St       | Anytown       | CA             | 04/03/2023     | 12/31/9999  | 1          |

### Type 3

This technique involves adding a new attribute to the existing record in the dimension table. The new attribute represents the latest value, while the existing attributes represent historical values. This approach maintains some historical data, while reducing the number of records in the dimension table. However, it does not maintain a complete history of the dimension.

In Type 3 SCDs, only one previous value is stored, which means that if the dimension changes a second time, the previous value is overwritten and lost. This technique is useful for scenarios where the dimension changes infrequently and the organization only needs to store one previous value.

#### Example

Before:

| customer_key | customer_id | customer_name | customer_address | customer_city | customer_state |
| ------------ | ----------- | ------------- | ---------------- | ------------- | -------------- |
| 1            | 123         | John Smith    | 123 Main St      | Anytown       | CA             |

After a first change:

| customer_key | customer_id | customer_name | customer_address | customer_city | customer_state | customer_name_new |
| ------------ | ----------- | ------------- | ---------------- | ------------- | -------------- | ----------------- |
| 1            | 123         | John Smith    | 123 Main St      | Anytown       | CA             | Jane Doe          |

After a second change:

| customer_key | customer_id | customer_name | customer_address | customer_city | customer_state | customer_name_new |
| ------------ | ----------- | ------------- | ---------------- | ------------- | -------------- | ----------------- |
| 1            | 123         | Jane Doe      | 123 Main St      | Anytown       | CA             | Jack Dawson       |

## Note on primary keys

It is considered a best practice to have a primary key in all tables, even if there is a unique combination of non-unique keys that could serve as an alternate key. The primary key is a fundamental concept in relational databases, and it ensures that each row in the table is uniquely identifiable. It also allows for efficient querying and indexing of the data.
