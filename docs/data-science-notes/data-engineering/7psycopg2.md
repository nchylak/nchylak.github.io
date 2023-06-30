---
layout: default
title: Psycopg2
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/7psycopg2/
nav_order: 7
---

# Psycopg2

## Basic usage

```python
import psycopg2

# Connect to an existing database
conn = psycopg2.connect("dbname=test user=postgres")

# Open a cursor to perform database operations
cur = conn.cursor()

# Execute a command: this creates a new table
cur.execute("CREATE TABLE test (id serial PRIMARY KEY, num integer, data varchar);")

# Pass data to fill a query placeholders and let Psycopg perform
# the correct conversion (no more SQL injections!)
cur.execute("INSERT INTO test (num, data) VALUES (%s, %s);",(100, "abc'def"))

# Query the database and obtain data as Python objects
cur.execute("SELECT * FROM test;")
cur.fetchone()
#(1, 100, "abc'def")

# Make the changes to the database persistent
conn.commit()

# Close communication with the databasecur.close()
conn.close()
```

## Committing

Committing makes changes to the database persistent. There are three ways of committing transactions:

1. Call `conn.commit()` at the end of all transactions (after 1 or more `cur.execute()`)
2. Turn autocommit on

```python
conn.autocommit = True #OR
conn.set_session(autocommit=True)
```

3. Use with

```python
with conn, conn.cursor() as cur:  # start a transaction and create a cursor
    cur.execute(sql)
```
