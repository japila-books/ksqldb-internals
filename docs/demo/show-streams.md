# Demo: SHOW STREAMS

`SHOW STREAMS` (`LIST STREAMS`) is a metadata query.

This demo shows the way of `SHOW STREAMS` query from [ksql CLI](../cli/index.md), through [ksqlDB API server](../rest/KsqlRestApplication.md), executing it on [KsqlEngine](../KsqlEngine.md) and returning a response back.

```text
ksql> SHOW STREAMS;

 Stream Name         | Kafka Topic                 | Key Format | Value Format | Windowed
------------------------------------------------------------------------------------------
 KSQL_PROCESSING_LOG | default_ksql_processing_log | KAFKA      | JSON         | false
------------------------------------------------------------------------------------------
```

`SHOW STREAMS` is parsed using [AstBuilder.Visitor](../parser/AstBuilder.Visitor.md#visitListStreams) into a [ListStreams](../parser/ListStreams.md) that is in turn executed using [ListSourceExecutor](../rest/ListSourceExecutor.md#streams).

## Create New Stream

```sql
CREATE STREAM demo_list_streams
(
    id BIGINT,
    name STRING
)
WITH
(
    kafka_topic='demo_list_streams',
    value_format='JSON',
    partitions=1
);
```

```text
ksql> SHOW STREAMS;

 Stream Name         | Kafka Topic                 | Key Format | Value Format | Windowed
------------------------------------------------------------------------------------------
 DEMO_LIST_STREAMS   | demo_list_streams           | KAFKA      | JSON         | false
 KSQL_PROCESSING_LOG | default_ksql_processing_log | KAFKA      | JSON         | false
------------------------------------------------------------------------------------------
```

```text
ksql> SELECT * FROM demo_list_streams;
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|ID                                                                       |NAME                                                                     |
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
Query Completed
Query terminated
```

```shell
echo '0:{"id":0, "name": "Demo"}' | kcat -P -b :9092 -t demo_list_streams -K :
```

```text
ksql> SELECT * FROM demo_list_streams;
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|ID                                                                       |NAME                                                                     |
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|0                                                                        |Demo                                                                     |
Query Completed
Query terminated
```

```text
ksql> EXPLAIN SELECT * FROM demo_list_streams;

ID                   : transient_DEMO_LIST_STREAMS_2530336553886215962
Query Type           : PUSH
SQL                  : SELECT * FROM demo_list_streams;

 Field | Type
-------------------------
 ID    | BIGINT
 NAME  | VARCHAR(STRING)
-------------------------

Sources that this query reads from:
-----------------------------------
DEMO_LIST_STREAMS

For source description please run: DESCRIBE [EXTENDED] <SourceId>

Execution plan
--------------
 > [ PROJECT ] | Schema: ID BIGINT, NAME STRING | Logger: transient_DEMO_LIST_STREAMS_2530336553886215962.Project
   > [ SOURCE ] | Schema: ID BIGINT, NAME STRING, ROWTIME BIGINT, ROWPARTITION INTEGER, ROWOFFSET BIGINT | Logger: transient_DEMO_LIST_STREAMS_2530336553886215962.KsqlTopic.Source


Processing topology
-------------------
Topologies:
   Sub-topology: 0
    Source: KSTREAM-SOURCE-0000000000 (topics: [demo_list_streams])
      --> KSTREAM-TRANSFORMVALUES-0000000001
    Processor: KSTREAM-TRANSFORMVALUES-0000000001 (stores: [])
      --> Project
      <-- KSTREAM-SOURCE-0000000000
    Processor: Project (stores: [])
      --> KSTREAM-PROCESSOR-0000000003
      <-- KSTREAM-TRANSFORMVALUES-0000000001
    Processor: KSTREAM-PROCESSOR-0000000003 (stores: [])
      --> none
      <-- Project
```

## Clean Up

```text
ksql> DROP STREAM demo_list_streams DELETE TOPIC;

 Message
--------------------------------------------------------------------
 Source `DEMO_LIST_STREAMS` (topic: demo_list_streams) was dropped.
--------------------------------------------------------------------
```
