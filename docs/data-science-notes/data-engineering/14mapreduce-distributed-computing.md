---
layout: default
title: MapReduce and distributed computing
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/14mapreduce-distributed-computing/
nav_order: 14
---

# MapReduce and distributed computing

## Distributed system

A distributed system is a computing environment (a **cluster**) in which various components are spread across multiple computers (called **nodes**) on a network. These computers split up the work, coordinating their efforts to complete the job more efficiently than if a single computer had been responsible for the task.

Usually, distributed systems are organised into a master worker hierarchy, meaning there is a **master node** responsible for orchestrating the tasks across the cluster.

## Parallel vs distrubuted computing

- In parallel computing, all processors may have access to a shared memory to exchange information between processors.
- In distributed computing, each processor has its own private memory. Information is exchanged by passing messages between the processors.

## MapReduce

MapReduce is a programming technique for manipulating large data sets. "Hadoop MapReduce" is a specific implementation of this programming technique.

The technique works by:

1. **Map**: first dividing up a large dataset and distributing the data across a cluster. In the map step, each data is analyzed and converted into a (key, value) pair.
2. **Shuffle**: Then these key-value pairs are shuffled across the cluster so that all keys are on the same machine. 
3. **Reduce**: In the reduce step, the values with the same keys are combined together.

While Spark doesn't implement MapReduce, Spark programs can be written so at to behave in a similar way to the map-reduce paradigm.
