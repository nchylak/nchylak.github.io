---
layout: default
title: Apache Airflow
parent: Data engineering
grand_parent: Data science notes
permalink: /docs/data-science-notes/data-engineering/19apache-airflow/
nav_order: 19
---

# Apache Airflow

## Data pipeline definitions

* **Data pipeline**: A series of steps in which data is processed.

- **Directed Acyclic Graphs (DAGs):** DAGs are a special subset of graphs in which the edges between nodes have a specific direction (**directed**), and no cycles exist (**acyclic**). When we say “no cycles exist” what we mean is the nodes cant create a path back to themselves.
- **Nodes:** A step in the data pipeline process.
- **Edges:** The dependencies or relationships other between nodes.
- Data lineage: This is like the genealogy tree of data, i.e. where the data comes from and how it was calculated. The steps and code used for transforming data can be seen in Airflow but, be careful, Airflow only shows the CURRENT version of it (i.e. you would see the current version also for historical runs) so think carefuly before changing that code. It may be better to put an end_date to a DAG and start a new one.

## Airflow components

- **Scheduler** orchestrates the execution of jobs on a trigger or schedule. The Scheduler chooses how to prioritize the running and execution of tasks within the system.
- **Work Queue** is used by the scheduler in most Airflow installations to deliver tasks that need to be run to the **Workers**.
- **Worker** processes execute the operations defined in each DAG. In most Airflow installations, workers pull from the work queue when it is ready to process a task. When the worker completes the execution of the task, it will attempt to process more work from the work queue until there is no further work remaining. When work in the queue arrives, the worker will begin to process it.
- **Database** saves credentials, connections, history, and configuration. The database, often referred to as the metadata database, also stores the state of all tasks in the system. Airflow components interact with the database with the Python ORM, SQLAlchemy.
- **Web Interface** provides a control dashboard for users and maintainers.

## Scheduling in Airflow

* **Start dates**: When chosing a `start_date` in the past and a `schedule_interval` resulting in multiple runs up until now, Airflow will **backfill** by creating a DAG run for every period between the `start_date` and now. In that case, you can control how many runs can take place at the same time with `max_active_runs`. This can be helpful when a run relies on the data produced by a previous run (in that case you would set `max_active_runs=1`). `start_date` is obligatory.
* **Schedule intervals**: `schedule_interval` determines how frequently a task will run from the start date. Syntax is "@daily", "@monthly", etc. or a cron expression. This parameter is optional.
* **End dates**: An `end_date` can also be set in order for a pipeline to stop running (rather than editing them to start doing something else or deleting them). This parameter is optional.

> **WARNING** If you clear a run, Airflow will still consider it scheduled (but missing) and thus will run it again!

## Partitioning in Airflow

#### Advantages

* Easier to debug: Tasks fail more quickly and you would know what - small - part of the pipeline is concerned
* Less time and cost spent on re-running failed tasks
* Faster overall: Airflow is able to parallelise the execution of unrelated jobs

#### Types

You can partition by: 

* Size: based on the desired or required storage limit
* Location: based on its location in a datastore
* Time: based on a schedule or when it was created
* Logical partitioning: based on conceptual attributes

## Data quality

Examples: 

- Data must be a certain size: E.g. check that an output table has at least 1 row
- Data must be accurate to some margin of error: E.g. check that min, max and average are within expected values
- Pipelines must run on a particular schedule: E.g. Ensure that some jobs finish running before others start
- Data must not contain any sensitive information: E.g. Remove PII
- Ensure data is processed within a certain timeframe: E.g. Stop the job if it is not finished within 1 hour

## Operators & Hooks

The Airflow community continuously contributes to creating new operators and hooks. These can be found [here](https://github.com/apache/airflow/tree/main/airflow/contrib). Always check contributions prior to creating your own things.

## Airflow syntax

### Admin/Connections

For storing credentials and have them encrypted by Airflow. Credentials are used in **hooks**, i.e. reusable connections to external databases or systems.

```python
from airflow.hooks.S3_hook import S3Hook

hook = S3Hook(aws_conn_id="aws_connection_name")
```

### Admin/Variables

For storing variables (similar to a constants.py file or a configuration file).

```python
# Importing these variables in a script
from airflow.models import Variable

my_var = Variable.get("variable_name")
```

- s3_bucket: udacity-dend
- s3_prefix: data-pipelines

### DAGs

```python
from airflow import DAG
import datetime

dag = DAG(
	"name_of_dag",
	schedule_interval="@daily",
	start_date=datetime.datetime.now())
```

If tasks within a DAG are very similar and share a lot of code together, they can be replaced by **SubDAGs** and the use of `SubDagOperator`s.

```python
from airflow import DAG
from airflow.operators.postgres_operator import PostgresOperator
from airflow.operators.udacity_plugin import S3ToRedshiftOperator
from airflow.operators.subdag_operator import SubDagOperator

import sql_statements

def get_s3_to_redshift_dag(
        parent_dag_name,
        task_id,
        redshift_conn_id,
        aws_credentials_id,
        table,
        create_sql_stmt,
        s3_bucket,
        s3_key,
        *args, **kwargs):
    dag = DAG(
        f"{parent_dag_name}.{task_id}",
        **kwargs
    )

    create_task = PostgresOperator(
        task_id=f"create_{table}_table",
        dag=dag,
        postgres_conn_id=redshift_conn_id,
        sql=create_sql_stmt
    )

    copy_task = S3ToRedshiftOperator(
        task_id=f"load_{table}_from_s3_to_redshift",
        dag=dag,
        table=table,
        redshift_conn_id=redshift_conn_id,
        aws_credentials_id=aws_credentials_id,
        s3_bucket=s3_bucket,
        s3_key=s3_key
    )

    create_task >> copy_task

    return dag
  
dag = DAG(
    "parent_dag",
    start_date=start_date,
)

trips_task_id = "trips_subdag"
trips_subdag_task = SubDagOperator(
    subdag=get_s3_to_redshift_dag(
        "parent_dag",
        trips_task_id,
        "redshift",
        "aws_credentials",
        "trips",
        sql_statements.CREATE_TRIPS_TABLE_SQL,
        s3_bucket="my_bucket",
        s3_key="path/to/file1.csv",
        start_date=start_date,
    ),
    task_id=trips_task_id,
    dag=dag,
)

stations_task_id = "stations_subdag"
stations_subdag_task = SubDagOperator(
    subdag=get_s3_to_redshift_dag(
        "parent_dag",
        stations_task_id,
        "redshift",
        "aws_credentials",
        "stations",
        sql_statements.CREATE_STATIONS_TABLE_SQL,
        s3_bucket="udacity-dend",
        s3_key="path/to/file2.csv",
        start_date=start_date,
    ),
    task_id=stations_task_id,
    dag=dag,
)

trips_subdag_task >> further_tasks
stations_subdag_task >> further_tasks
```

### PythonOperator

```python
from airflow.operators.python_operator import PythonOperator

def my_function(x):
	return y

my_task = PythonOperator(
	task_id="task_name",
	python_callable=my_function,
	dag=dag)
```

For executing SQL queries in Postgres or Redshift

```python
from airflow.contrib.hooks.aws_hook import AwsHook
from airflow.hooks.postgres_hook import PostgresHook
from airflow.operators.python_operator import PythonOperator
import queries # queries.py with queries saved

def load_data_to_redshift(*args, **kwargs):
    aws_hook = AwsHook("aws_credentials")
    aws_credentials = aws_hook.get_credentials()
    redshift_hook = PostgresHook("redshift")
    redshift_hook.run(queries.COPY_QUERY.format(aws_credentials.access_key,
                                              aws_credentials.secret_key))

copy_task = PythonOperator(
    task_id='load_from_s3_to_redshift',
    dag=dag,
    python_callable=load_data_to_redshift
)
```

### PostgresOperator

For executing SQL queries in Postgres or Redshift

```python
from airflow.operators.postgres_operator import PostgresOperator
import queries # queries.py with queries saved

create_table = PostgresOperator(
    task_id="create_table",
    dag=dag,
    postgres_conn_id="redshift",
    sql=sql_statements.CREATE_TABLE_QUERY
)
```

### Task dependencies

```python
#                -> task_2
#              /           \
#   start_task               -> final_task
#              \           /
#                -> task_3

start_task >> task_2
start_task >> task_3
task_2 >> final_task
task_3 >> final_task
```

### Context variables

https://airflow.apache.org/docs/apache-airflow/stable/templates-ref.html#variables

### Data partitioning

```python
COPY_SQL = """
COPY {}
FROM '{}'
ACCESS_KEY_ID '{{}}'
SECRET_ACCESS_KEY '{{}}'
IGNOREHEADER 1
DELIMITER ','
"""

COPY_MONTHLY_TRIPS_SQL = COPY_SQL.format(
    "trips",
    "s3://udacity-dend/data-pipelines/divvy/partitioned/{year}/{month}/divvy_trips.csv"
) # the first set of {} is removed from {{}} as no arg is provided

def load_trip_data_to_redshift(*args, **kwargs):
    aws_hook = AwsHook("aws_credentials")
    credentials = aws_hook.get_credentials()
    redshift_hook = PostgresHook("redshift")

		execution_date=kwargs["execution_date"] # see above: Context variables

    sql_stmt = COPY_MONTHLY_TRIPS_SQL.format(
        credentials.access_key,
        credentials.secret_key,
        year=execution_date.year,
        month=execution_date.month
    )
    redshift_hook.run(sql_stmt)
    
copy_trips_task = PythonOperator(
    task_id='load_trips_from_s3_to_redshift',
    dag=dag,
    python_callable=load_trip_data_to_redshift,
    provide_context=True
)
```

In practice, it is often best to have Airflow process pre-partitioned data. If your upstream data sources cannot partition data, it is possible to write an Airflow DAG to partition the data. However, it is worth keeping in mind memory limitations on your Airflow workers. If the size of the data to be partitioned exceeds the amount of memory available on your worker, the DAG will not successfully execute.

### Data quality

#### Size check

```python
def check_greater_than_zero(*args, **kwargs):
    table = kwargs["params"]["table"]
    redshift_hook = PostgresHook("redshift")
    records = redshift_hook.get_records(f"SELECT COUNT(*) FROM {table}")
    if len(records) < 1 or len(records[0]) < 1:
        raise ValueError(f"Data quality check failed. {table} returned no results")
    num_records = records[0][0]
    if num_records < 1:
        raise ValueError(f"Data quality check failed. {table} contained 0 rows")
    logging.info(f"Data quality on table {table} check passed with {records[0][0]} records")
    
check_trips = PythonOperator(
    task_id='check_trips_data',
    dag=dag,
    python_callable=check_greater_than_zero,
    provide_context=True,
    params={'table': 'trips'}
)
```

#### Time out rule

Use `sla` with a timedelta:

```python
my_task = PythonOperator(
    task_id='my_task',
    dag=dag,
    python_callable=my_function,
		sla=datetime.timedelta(hours=1)
)
```

## Command line interface

### Run a task

```bash
airflow run <dag_id> <task_id> <start_date>
```

### General commands

```
airflow list_dags
airflow -h # get help
airflow <subcommand> -h # get help on a subcommand
```
