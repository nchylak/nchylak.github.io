---
layout: default
title: Redshift DML
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/10aws-redshift-dml/
nav_order: 10
---

# Redshift DML

## COPY

* It is faster to use the `COPY` command and not the `INSERT` command as with `INSERT` rows are inserted one by one.

* `COPY` on the other hand can process an entire file and even multiple files in parallel:
  * Using a common prefix

    ```sql
    COPY table_name FROM 's3://bucket/folder/part'
    CREDENTIALS 'aws_iam_role::12345:role/myRole'
    GZIP DELIMITER ';' REGION 'us-west-2';
    ```

    This will copy `part-01.csv.gz`, `part-02.csv.gz`, etc.

  * Using a manifest file

    ```sql
    COPY table_name FROM 's3://bucket/mydata.manifest'
    IAM_ROLE 'arn:aws:iam:12345:role/myRole'
    MANIFEST;
    ```

    With `mydata.manifest` as follows:

    ```json
    {
    	"entries": [
    		{"url":"s3://bucket/file_name1", "mandatory":true},
    		{"url":"s3://bucket/file_name2", "mandatory":true},
    		{"url":"s3://bucket/file_name3", "mandatory":true}
    	]
    }
    ```

* It is better to ingest from the same AWS region as the cluster.

* It is better to compress the files, which will be processed.

## UNLOAD

* For getting data of Redshift, Redshift does not use `COPY TO` but `UNLOAD`

```sql
UNLOAD ('select * from a_table')
TO 's3://bucket/file_'
IAM_ROLE 'arn:aws:iam:12345:role/myRole'
```
