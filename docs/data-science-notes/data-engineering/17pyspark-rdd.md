---
layout: default
title: PySpark RDDs
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/17pyspark-rdd/
nav_order: 17
---

# PySpark RDDs

RDDs are a low-level abstraction of the data. In the first version of Spark, you worked directly with RDDs. You can think of RDDs as long lists distributed across various machines. You can still use RDDs as part of your Spark code although data frames and SQL are easier.

Unlike the data frame, where most of manipulations revolve around columns, RDD revolve around *objects*. You can think of an RDD as a bag of elements with no order or relationship to one another. Each element is independent of the other.

For this reason, RDD is much more tolerant in terms of what it accepts. If we were trying to store an integer, a string and a dictionary in a single column, the data frame would fail to find a common denominator to fit those different data types.

In RDDs, data is divided into logical partitions, which may be computed on different nodes of the cluster.

### Convert a list to an RDD: `.parallelize()`

```
rdd = sparkContext.parallelize([1,2,3,4,5])
```

Let's focus on three higher-order functions (functions that take other functions as parameters) applicable to RDDs.

#### Apply one function to every object: `map()`
