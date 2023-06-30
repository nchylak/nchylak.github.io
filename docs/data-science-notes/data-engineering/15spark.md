---
layout: default
title: Spark
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/15spark/
nav_order: 15
---

# Spark

## Spark vs Hadoop

The Hadoop ecosystem is a slightly older technology than the Spark ecosystem. In general, Hadoop MapReduce is slower than Spark because Hadoop writes data out to disk during intermediate steps. However, many big companies, such as Facebook and LinkedIn, started using Big Data early and built their infrastructure around the Hadoop ecosystem.

While Spark is great for iterative algorithms, there is not much of a performance boost over Hadoop MapReduce when doing simple counting. Migrating legacy code to Spark, especially on hundreds of nodes that are already in production, might not be worth the cost for the small performance boost.

## PySpark vs Pandas

Spark is meant for big datasets that cannot fit on one computer. If a dataset fits in memory, it may be more efficient to use pandas instead.

## Spark's modes

* **Local Mode**: No cluster manager. Everything happens on a single machine (no distribution). Used for prototyping, learning Spark's syntax, etc.

* **Standalone Mode**: Spark's own cluster manager

* **YARN Mode**: Cluster manager from the Hadoop project

* **Mesos Mode**: Open source cluster manager from UC Berkeley

## Spark's limitations

Streaming: Native streaming tools such as Storm, Apex or Flink can be doing better

Machine Learning: Spark only supports algorithms that scale linearly with the input data size. In general, deep learning is not available either.

## Spark's ports

* Port `4040` is the port which shows active Spark jobs
* Port `8080` is the port to Spark's web UI
* Port `7077` is used by the master node to communicate with the worker nodes

## Spark's webUI

The webUI provides:

* the status of the cluster
* the cluster's configuration
* the status of recent jobs

## Data skewness

A job works fine on a small amount of data but fails when data scales up because a worker (or a few workers) is overloaded with data. This is because data is paritioned in a way that, for example, 1 out of 5 workers (so 20%) receives 80% of the work.

This is suboptimal because the other workers are idle while they wait for the job of the overloaded workers to be finished.

You can identify data skewness via the web UI.

There are several ways of making up for data skewness:

* Choose a different partition key
* Create a composite key to partition by
* Use `spark_DF.repartition(nb_workers)` or `spark_DF.repartition(column_name)`.
