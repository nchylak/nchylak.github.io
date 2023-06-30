---
layout: default
title: PostgreSQL DDL
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/5postgresql-ddl/
nav_order: 5
---

# PostgreSQL DDL

The Data Definition Language (DDL) consists of the following statements:

- CREATE
- ALTER
- TRUNCATE
- DROP

## CREATE

### Basic syntax

```sql
CREATE TABLE <table_name> (
  column_name_1 datatype,
  column_name_2 datatype,
  ...
  ...
  column_name_n datatype
);
```

### Data Definition and Constraints

The CREATE statement in SQL has a few important constraints that are highlighted below.

#### NOT NULL

The **NOT NULL** constraint indicates that the column cannot contain a null value.

Here is the syntax for adding a NOT NULL constraint to the CREATE statement:

```python
CREATE TABLE IF NOT EXISTS customer_transactions (
    customer_id int NOT NULL, 
    store_id int, 
    spent numeric
);
```

You can add **NOT NULL** constraints to more than one column. Usually this occurs when you have a **COMPOSITE KEY**, which will be discussed further below.

Here is the syntax for it:

```python
CREATE TABLE IF NOT EXISTS customer_transactions (
    customer_id int NOT NULL, 
    store_id int NOT NULL, 
    spent numeric
);
```

#### UNIQUE

The **UNIQUE** constraint is used to specify that the data across all the rows in one column are unique within the table. The **UNIQUE** constraint can also be used for multiple columns, so that the combination of the values across those columns will be unique within the table. In this latter case, **the values within 1 column do not need to be unique**.

Let's look at an example.

```python
CREATE TABLE IF NOT EXISTS customer_transactions (
    customer_id int NOT NULL UNIQUE, 
    store_id int NOT NULL UNIQUE, 
    spent numeric 
);
```

Another way to write a **UNIQUE** constraint is to add a table constraint using commas to separate the columns.

```python
CREATE TABLE IF NOT EXISTS customer_transactions (
    customer_id int NOT NULL, 
    store_id int NOT NULL, 
    spent numeric,
    UNIQUE (customer_id, store_id, spent)
);
```

#### PRIMARY KEY

The **PRIMARY KEY** constraint is defined on a single column, and every table should contain a primary key. The values in this column uniquely identify the rows in the table. If a group of columns are defined as a primary key, they are called a **composite key**. That means the combination of values in these columns will uniquely identify the rows in the table. By default, the **PRIMARY KEY** constraint has the **UNIQUE** and **NOT NULL** constraint built into it.

Let's look at the following example:

```python
CREATE TABLE IF NOT EXISTS store (
    store_id int PRIMARY KEY, 
    store_location_city text,
    store_location_state text
);
```

Here is an example for a group of columns serving as **composite key**.

```python
CREATE TABLE IF NOT EXISTS customer_transactions (
    customer_id int, 
    store_id int, 
    spent numeric,
    PRIMARY KEY (customer_id, store_id)
);
```

To read more about these constraints, check out the [PostgreSQL documentation](https://www.postgresql.org/docs/9.4/ddl-constraints.html).

## ALTER

There are a great variety of actions possible. It is better to refer to the [PostgreSQL documentation](https://www.postgresql.org/docs/9.1/sql-altertable.html).

```sql
ALTER TABLE [ ONLY ] name [ * ]
    action [, ... ]
ALTER TABLE [ ONLY ] name [ * ]
    RENAME [ COLUMN ] column TO new_column
ALTER TABLE name
    RENAME TO new_name
ALTER TABLE name
    SET SCHEMA new_schema

where action is one of:

    ADD [ COLUMN ] column data_type [ COLLATE collation ] [ column_constraint [ ... ] ]
    DROP [ COLUMN ] [ IF EXISTS ] column [ RESTRICT | CASCADE ]
    ALTER [ COLUMN ] column [ SET DATA ] TYPE data_type [ COLLATE collation ] [ USING expression ]
    ALTER [ COLUMN ] column SET DEFAULT expression
    ALTER [ COLUMN ] column DROP DEFAULT
    ALTER [ COLUMN ] column { SET | DROP } NOT NULL
    ALTER [ COLUMN ] column SET STATISTICS integer
    ALTER [ COLUMN ] column SET ( attribute_option = value [, ... ] )
    ALTER [ COLUMN ] column RESET ( attribute_option [, ... ] )
    ALTER [ COLUMN ] column SET STORAGE { PLAIN | EXTERNAL | EXTENDED | MAIN }
    ADD table_constraint [ NOT VALID ]
    ADD table_constraint_using_index
    VALIDATE CONSTRAINT constraint_name
    DROP CONSTRAINT [ IF EXISTS ]  constraint_name [ RESTRICT | CASCADE ]
    DISABLE TRIGGER [ trigger_name | ALL | USER ]
    ENABLE TRIGGER [ trigger_name | ALL | USER ]
    ENABLE REPLICA TRIGGER trigger_name
    ENABLE ALWAYS TRIGGER trigger_name
    DISABLE RULE rewrite_rule_name
    ENABLE RULE rewrite_rule_name
    ENABLE REPLICA RULE rewrite_rule_name
    ENABLE ALWAYS RULE rewrite_rule_name
    CLUSTER ON index_name
    SET WITHOUT CLUSTER
    SET WITH OIDS
    SET WITHOUT OIDS
    SET ( storage_parameter = value [, ... ] )
    RESET ( storage_parameter [, ... ] )
    INHERIT parent_table
    NO INHERIT parent_table
    OF type_name
    NOT OF
    OWNER TO new_owner
    SET TABLESPACE new_tablespace

and table_constraint_using_index is:

    [ CONSTRAINT constraint_name ]
    { UNIQUE | PRIMARY KEY } USING INDEX index_name
    [ DEFERRABLE | NOT DEFERRABLE ] [ INITIALLY DEFERRED | INITIALLY IMMEDIATE ]
```

## TRUNCATE

This is for removing all rows from a table (but keep the table as an "empty shell").

```sql
TRUNCATE TABLE <tableName>;
```

## DROP

This is for deleting a table together with the data stored in it.

```sql
DROP TABLE <tableName>;
```

Works also for dropping schemas, databases, etc.
