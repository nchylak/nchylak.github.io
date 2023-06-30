---
layout: default
title: PySpark
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/pyspark/
nav_order: 3
---

# PySpark

## General

### Functional programming

Python is an object oriented language but the PySpark API was written with functional programming principles in mind:

- Object-oriented languages are good when you have a **fixed set of operations on things**, and as your code evolves, you primarily **add new things**. This can be accomplished by adding new classes which implement existing methods, and the existing classes are left alone.
- Functional languages are good when you have a **fixed set of things**, and as your code evolves, you primarily **add new operations on existing things**. This can be accomplished by adding new functions which compute with existing data types, and the existing functions are left alone.

In Python, everything that comes after `def` is called a function, however, these more often resemble more methods and procedures. Real functions are like math functions (`f(X) = Y`) and are also called **pure functions**:

* Pure functions do not have any side effects on variables outside of their scope
* Pure functions do not alter the input data (aka mother data).

### Lazy execution

Every instruction in PySpark can be classified into two categories: 

1. **Transformations**: renaming a column, aggregating, etc. 
2. **Actions**: print on screen, write to a file, etc.

Pandas reads data into memory and performs all transformations AND actions as soon as they are specified. This is called **eager execution**. On the other hand, PySpark does not actually make any transformation until an action takes place. This is called **lazy execution**.

Upon transformation, PySpark builds a step-by-step plan of what functions and data it will need (a directed acyclical graph aka **DAG**) but waits until the last possible moment to put this plan into execution.

## SparkSession

> See 2.1.1 of Data Analysis with Python and PySpark

The `SparkSession` object is a more recent addition to the PySpark API since version 2.0 (before sessions had to start with a `SparkContext`). This is due to the API evolving in a way that makes more room for the faster, more versatile data frame as the main data structure over the lower level RDD.

```python
from pyspark.sql import SparkSession

spark = (SparkSession
         .builder
         .appName("A name for the app.")
         .getOrCreate())
```



---



Distinct on: 

* Groupby and max
* Dropduplicates() or drop duplicates([subset of columns]) but then you do not control which record get dropped (sort is not really a thing for spark as the dataset is distribued for processing so you would need to create a parittion first)

## Fill NA

```
df4.na.fill({'age': 50, 'name': 'unknown'})
```

#### User defined functions (UDF)

In Spark SQL we can define our own functions with the udf method from the pyspark.sql.functions module. The default type of the returned variable for UDFs is string. If we would like to return an other type we need to explicitly do so by using the different types from the pyspark.sql.types module.
