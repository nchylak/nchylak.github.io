---
layout: default
title: PostgreSQL DML
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/postgresql-dml/
nav_order: 3
---

# PostgreSQL DML

## Upsert

In RDBMS language, the term *upsert* refers to the idea of inserting a new row in an existing table, or updating the row if it already exists in the table. The action of updating or inserting has been described as "upsert".

The way this is handled in PostgreSQL is by using the `INSERT` statement in combination with the `ON CONFLICT` clause.

## INSERT

### General

The **INSERT** statement adds in new rows within the table. The values associated with specific target columns can be added in any order.

Let's use a customer address table as an example, which is defined with the following **CREATE** statement:

```python
CREATE TABLE IF NOT EXISTS customer_address (
    customer_id int PRIMARY KEY, 
    customer_street varchar NOT NULL,
    customer_city text NOT NULL,
    customer_state text NOT NULL
);
```

A new row is added as below:

```python
INSERT INTO customer_address (
VALUES
    (432, '758 Main Street', 'Chicago', 'IL'
);
```

### Conflicts

#### Do nothing

Let's assume that we have a second address for a customer. However we do not want to add a new customer id and decide it is better to keep the original address. In other words, if there is any conflict on the `customer_id`, we do not the INSERT statement to insert or edit any row.

This would be a good candidate for using the **ON CONFLICT DO NOTHING** clause.

```python
INSERT INTO customer_address (customer_id, customer_street, customer_city, customer_state)
VALUES
 (
 432, '785 Main Street', 'Chicago', 'IL'
 ) 
ON CONFLICT (customer_id) 
DO NOTHING;
```

#### Do update

If, on the contrary, we would like to update the customer's address, this would be a good candidate for using the **ON CONFLICT DO UPDATE** clause.

```python
INSERT INTO customer_address (customer_id, customer_street)
VALUES
    (
    432, '785 Main Street', 'Chicago', 'IL' 
		) 
ON CONFLICT (customer_id) 
DO UPDATE
    SET customer_street  = EXCLUDED.customer_street;
```

Check out these two links to learn other ways to insert data into the tables.

- [PostgreSQL tutorial](http://www.postgresqltutorial.com/postgresql-upsert/)
- [PostgreSQL documentation](https://www.postgresql.org/docs/9.5/sql-insert.html)

## UPDATE

The **UPDATE** command is used to modify existing data in a table. It is possible to update a single column, a single row, multiple columns, or multiple rows at a time. Let us take a look at the syntax:

```mssql
UPDATE table_name SET column1 = value1, column2 = value2,... columnn = valuen
WHERE condition;
```

In this statement, we first, specify the name of the table that you want to update data after the UPDATE keyword.

Second, the SET keyword is used to specify columns and their new values. If there are multiple columns to change, separate each column=value pair with a comma.

Next, the WHERE keyword is used to define condition to determine which row(s) to update for the column(s) already specified after the SET keyword. The WHERE keyword is optional and if omitted the UPDATE statement will update all rows in the table.

The UPDATE statement returns the count of the number of rows updated.

## DELETE

The **DELETE** command is used to delete data from a table in a database. Let us take a look at the syntax:

```mssql
DELETE FROM table_name    
WHERE condition;
```

In this statement, we specify the table name from which you want to delete data after the 'DELETE FROM' keyword. Next, use the 'WHERE' condition to determine rows from the table from which you want to delete data

The DELETE statement returns the count of the number of rows deleted.
