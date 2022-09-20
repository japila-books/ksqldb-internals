# Demo: Table isn't queryable

This demo shows the internals of this infamous error while querying a table that is not queryable whatsoever.

```text
ksql> SELECT * FROM my_table;
The `MY_TABLE` table isn't queryable. To derive a queryable table, you can do 'CREATE TABLE QUERYABLE_MY_TABLE AS SELECT * FROM MY_TABLE'. See https://cnfl.io/queries for more info.
Add EMIT CHANGES if you intended to issue a push query.
Statement: SELECT * FROM my_table;: SELECT * FROM my_table;
```

## Create Stream

=== "KSQL"

    ```sql
    CREATE STREAM my_stream (
        id INTEGER,
        name STRING)
    WITH (
        KAFKA_TOPIC='my_topic',
        VALUE_FORMAT='json',
        PARTITIONS=1);
    ```

```text
 Message
----------------
 Stream created
----------------
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

Send two messages to a Kafka cluster.

```shell
echo '1:{"id": 1, "name": "one"}' | kcat -P -b :9092 -t my_topic -K :
echo '1:{"id": 1, "name": "ONE"}' | kcat -P -b :9092 -t my_topic -K :
```

Query the stream to make sure that there are two records indeed.

=== "KSQL"

    ```sql
    SELECT * FROM my_stream;
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

You could instead use `PRINT [topic]` command.

=== "KSQL"

    ```sql
    PRINT my_topic FROM BEGINNING LIMIT 2;
    ```

```text
Key format: JSON or KAFKA_STRING
Value format: JSON or KAFKA_STRING
rowtime: 2022/09/20 19:28:31.292 Z, key: 1, value: {"id": 1, "name": "one"}, partition: 0
rowtime: 2022/09/20 19:28:31.324 Z, key: 1, value: {"id": 1, "name": "ONE"}, partition: 0
Topic printing ceased
```

## Create Table

Given these two records with the same key `1`, I thought I'd use a ksql table to turn them into a single record with the last values for `id` and `name` columns.

I created a table on the stream topic directly.

=== "KSQL"

    ```sql
    CREATE TABLE my_table (
        id STRING PRIMARY KEY,
        name STRING)
    WITH (
        KAFKA_TOPIC='my_topic',
        VALUE_FORMAT='json');
    ```

```text
 Message
---------------
 Table created
---------------
```

## Issue: Table Isn't Queryable

Execute the following KSQL query to reproduce the infamous `table isn't queryable` error.

=== "KSQL"

    ```sql
    SELECT * FROM my_table;
    ```

```text
The `MY_TABLE` table isn't queryable. To derive a queryable table, you can do 'CREATE TABLE QUERYABLE_MY_TABLE AS SELECT * FROM MY_TABLE'. See https://cnfl.io/queries for more info.
Add EMIT CHANGES if you intended to issue a push query.
Statement: SELECT * FROM my_table;: SELECT * FROM my_table;
```

Why?! It worked like a charm with a stream. Why does it not work with a table?!

## ksqlDB Internals

The exception is thrown when `EngineExecutor` is requested to [build a physical plan for a pull query](../EngineExecutor.md#buildPullPhysicalPlan) and [builds a PullPhysicalPlanBuilder](../PullPhysicalPlanBuilder.md).

There are two code paths that lead to [throwing a notMaterializedException](../PullQueryExecutionUtil.md#notMaterializedException), namely before and while [building a PullPhysicalPlanBuilder](../PullPhysicalPlanBuilder.md):

1. Before a `PullPhysicalPlanBuilder` is built, `EngineExecutor` [findMaterializingQuery](../PullQueryExecutionUtil.md#findMaterializingQuery) that can possibly [throw notMaterializedException](../PullQueryExecutionUtil.md#notMaterializedException)
1. While building a `PullPhysicalPlanBuilder`, the `PullPhysicalPlanBuilder` uses the [PersistentQueryMetadata](../PersistentQueryMetadata.md) found to [getMaterialization](../PersistentQueryMetadata.md#getMaterialization). Unless the materialization is found, [a notMaterializedException is thrown](../PullQueryExecutionUtil.md#notMaterializedException)

!!! note "#9571 Infamous Table isn't queryable error (and possible code duplication)"
    How is this possible that [findMaterializingQuery](../PullQueryExecutionUtil.md#findMaterializingQuery) is successful while [getMaterialization](../PersistentQueryMetadata.md#getMaterialization) will not?

    I asked this question in [#9571](https://github.com/confluentinc/ksql/issues/9571).

## Solution: Create Source Table

It turns out that a solution is to `CREATE SOURCE TABLE` (not `CREATE TABLE`).

=== "KSQL"

    ```sql
    CREATE SOURCE TABLE my_source_table (
        id STRING PRIMARY KEY,
        name STRING)
    WITH (
        KAFKA_TOPIC='my_topic',
        VALUE_FORMAT='json');
    ```

```text
 Message
---------------------------------------------
 Created query with ID CST_MY_SOURCE_TABLE_5
---------------------------------------------
```

Execute the KSQL query that, this time, should work just fine.

=== "KSQL"

    ```sql
    SELECT * FROM my_source_table;
    ```

```text
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|ID                                                                       |NAME                                                                     |
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|1                                                                        |ONE                                                                      |
Query terminated
```

### Source Table Explained

=== "KSQL"

    ```sql
    DESCRIBE my_source_table EXTENDED;
    ```

```text
Name                 : MY_SOURCE_TABLE
Type                 : TABLE
Timestamp field      : Not set - using <ROWTIME>
Key format           : KAFKA
Value format         : JSON
Kafka topic          : my_topic (partitions: 1, replication: 1)
Statement            : CREATE SOURCE TABLE MY_SOURCE_TABLE (ID STRING PRIMARY KEY, NAME STRING) WITH (KAFKA_TOPIC='my_topic', KEY_FORMAT='KAFKA', VALUE_FORMAT='JSON');

 Field | Type
----------------------------------------
 ID    | VARCHAR(STRING)  (primary key)
 NAME  | VARCHAR(STRING)
----------------------------------------

Queries that read from this TABLE
-----------------------------------
CST_MY_SOURCE_TABLE_5 (RUNNING) : CREATE SOURCE TABLE MY_SOURCE_TABLE (ID STRING PRIMARY KEY, NAME STRING) WITH (KAFKA_TOPIC='my_topic', KEY_FORMAT='KAFKA', VALUE_FORMAT='JSON');

For query topology and execution plan please run: EXPLAIN <QueryId>

Runtime statistics by host
-------------------------
 Host           | Metric                    | Value      | Last Message
------------------------------------------------------------------------------------
 localhost:8088 | consumer-messages-per-sec |          0 | 2022-09-20T19:35:24.371Z
 localhost:8088 | consumer-total-bytes      |         50 | 2022-09-20T19:35:24.371Z
 localhost:8088 | consumer-total-messages   |          2 | 2022-09-20T19:35:24.371Z
------------------------------------------------------------------------------------
(Statistics of the local KSQL server interaction with the Kafka topic my_topic)
```

## Follow-Up

One thing that may have caught your attention is the type of the primary key, i.e. `ID STRING PRIMARY KEY`.

The reason for this choice is that the demo uses `kcat` to send records and, as it turns out, keys cannot be any other type but `STRING`. Learn more in [Demo: Dealing with Deserialization Errors](dealing-with-deserialization-errors.md).

## Learn More

* [How to convert a changelog to a table](https://docs.ksqldb.io/en/latest/how-to-guides/convert-changelog-to-table/)
