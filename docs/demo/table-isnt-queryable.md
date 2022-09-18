# Demo: Table isn't queryable

This demo shows the internals of this infamous error while querying a table that is not queryable whatsoever.

```text
ksql> select * from my_table;
The `MY_TABLE` table isn't queryable. To derive a queryable table, you can do 'CREATE TABLE QUERYABLE_MY_TABLE AS SELECT * FROM MY_TABLE'. See https://cnfl.io/queries for more info.
Add EMIT CHANGES if you intended to issue a push query.
Statement: select * from my_table;: select * from my_table;
```

## Create Stream

Create a stream and send two records to make sure that it works just fine.

=== "KSQL"
    ```sql
    CREATE STREAM my_stream (
        id INTEGER,
        name STRING)
    WITH (
        kafka_topic='my_topic',
        value_format='json',
        partitions=1);
    ```

```text
ksql> LIST STREAMS;

 Stream Name         | Kafka Topic                 | Key Format | Value Format | Windowed
------------------------------------------------------------------------------------------
 KSQL_PROCESSING_LOG | default_ksql_processing_log | KAFKA      | JSON         | false
 MY_STREAM           | my_topic                    | KAFKA      | JSON         | false
------------------------------------------------------------------------------------------
```

In a terminal, execute the following command:

```shell
kcat -C -b :9092 -t my_topic
```

In another terminal, execute the following commands that sends two messages to a Kafka cluster.

```shell
echo '1:{"id": 1, "name": "one"}' | kcat -P -b :9092 -t my_topic -K :
echo '1:{"id": 1, "name": "ONE"}' | kcat -P -b :9092 -t my_topic -K :
```

=== "KSQL"
    ```sql
    select * from my_stream;
    ```

```text
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|ID                                                                       |NAME                                                                     |
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|1                                                                        |one                                                                      |
|1                                                                        |ONE                                                                      |
Query Completed
Query terminated
```

You should see two records. All should be working as expected.

## Create Table

This step will create a table on the stream topic (that, as you may have experienced already, makes a table non-queryable).

=== "KSQL"
    ```sql
    CREATE TABLE my_table (
        id INTEGER PRIMARY KEY,
        name STRING)
    WITH (
        kafka_topic='my_topic',
        value_format='json');
    ```

## Issue: Table Isn't Queryable

Execute the following KSQL query to reproduce the error.

=== "KSQL"
    ```sql
    select * from my_table;
    ```

```text
The `MY_TABLE` table isn't queryable. To derive a queryable table, you can do 'CREATE TABLE QUERYABLE_MY_TABLE AS SELECT * FROM MY_TABLE'. See https://cnfl.io/queries for more info.
Add EMIT CHANGES if you intended to issue a push query.
Statement: select * from my_table;: select * from my_table;
```

Why?! It worked like a charm with a stream, but does not with a table?!

## ksqlDB Internals

The exception is thrown when `EngineExecutor` is requested to [build a physical plan for a pull query](../EngineExecutor.md#buildPullPhysicalPlan) and [builds a PullPhysicalPlanBuilder](../PullPhysicalPlanBuilder.md).

There are two code paths that lead to [throwing a notMaterializedException](../PullQueryExecutionUtil.md#notMaterializedException), namely before and while [building a PullPhysicalPlanBuilder](../PullPhysicalPlanBuilder.md):

1. Before a `PullPhysicalPlanBuilder` is built, `EngineExecutor` [findMaterializingQuery](../PullQueryExecutionUtil.md#findMaterializingQuery) that can possibly [throw notMaterializedException](../PullQueryExecutionUtil.md#notMaterializedException)
1. While building a `PullPhysicalPlanBuilder`, the `PullPhysicalPlanBuilder` uses the [PersistentQueryMetadata](../PersistentQueryMetadata.md) found to [getMaterialization](../PersistentQueryMetadata.md#getMaterialization). Unless the materialization is found, [a notMaterializedException is thrown](../PullQueryExecutionUtil.md#notMaterializedException)

!!! note "#9571 Infamous Table isn't queryable error (and possible code duplication)"
    How is this possible that [findMaterializingQuery](../PullQueryExecutionUtil.md#findMaterializingQuery) is successful while [getMaterialization](../PersistentQueryMetadata.md#getMaterialization) will not?

    I asked this question in [#9571](https://github.com/confluentinc/ksql/issues/9571).

## Solution: Create Source Table

It turns out that a solution is to create a `SOURCE TABLE` instead.

=== "KSQL"

    ```sql
    CREATE SOURCE TABLE my_source_table (
        id INTEGER PRIMARY KEY,
        name STRING)
    WITH (
        kafka_topic='my_topic',
        value_format='json');
    ```

Execute the KSQL query that, this time, should work just fine.

=== "KSQL"

    ```sql
    select * from my_source_table;
    ```

### Source Table Explained

=== "KSQL"

    ```sql
    describe my_source_table extended;
    ```

```text
Name                 : MY_SOURCE_TABLE
Type                 : TABLE
Timestamp field      : Not set - using <ROWTIME>
Key format           : KAFKA
Value format         : JSON
Kafka topic          : my_topic (partitions: 1, replication: 1)
Statement            : CREATE SOURCE TABLE MY_SOURCE_TABLE (ID INTEGER PRIMARY KEY, NAME STRING) WITH (KAFKA_TOPIC='my_topic', KEY_FORMAT='KAFKA', VALUE_FORMAT='JSON');

 Field | Type
----------------------------------------
 ID    | INTEGER          (primary key)
 NAME  | VARCHAR(STRING)
----------------------------------------

Queries that read from this TABLE
-----------------------------------
CST_MY_SOURCE_TABLE_21 (RUNNING) : CREATE SOURCE TABLE MY_SOURCE_TABLE (ID INTEGER PRIMARY KEY, NAME STRING) WITH (KAFKA_TOPIC='my_topic', KEY_FORMAT='KAFKA', VALUE_FORMAT='JSON');

For query topology and execution plan please run: EXPLAIN <QueryId>

Runtime statistics by host
-------------------------
 Host           | Metric                           | Value      | Last Message
-------------------------------------------------------------------------------------------
 localhost:8088 | consumer-failed-messages         |          2 | 2022-09-18T11:27:17.437Z
 localhost:8088 | consumer-failed-messages-per-sec |          0 | 2022-09-18T11:27:17.437Z
 localhost:8088 | consumer-messages-per-sec        |          0 | 2022-09-18T11:27:17.436Z
 localhost:8088 | consumer-total-bytes             |         50 | 2022-09-18T11:27:17.436Z
 localhost:8088 | consumer-total-messages          |          2 | 2022-09-18T11:27:17.436Z
-------------------------------------------------------------------------------------------
(Statistics of the local KSQL server interaction with the Kafka topic my_topic)
```

## Learn More

* [How to convert a changelog to a table](https://docs.ksqldb.io/en/latest/how-to-guides/convert-changelog-to-table/)
