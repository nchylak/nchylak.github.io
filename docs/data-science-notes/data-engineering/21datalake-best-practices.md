---
layout: default
title: Data lake best practices
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/21datalake-best-practices/
nav_order: 21
---

# Data lake best practices

## S3 structure

When you identify an object in S3, you need to specify a bucket and key. For the example for the below object: 

```
s3://my_bucket/path/to/file/file.csv
```

* `s3://my_bucket/` is the bucket; and
* `path/to/file/file.csv` is the key.

## Ensure that all objects that share a path also share their schema

In Spark, it is possible to read in one single data frame all objects underneath a bucket if their share their path AND their schema. You can do so as follows:

```python
df = spark.read.load(“s3://my_bucket/path/to/file/”)
```
