# Demo: Pull Queries

This demo shows a [pull query](../pull-queries.md) in action.

## CREATE STREAM riderLocations

Use [ksql](../cli/index.md) to execute the following [CREATE STREAM](../parser/AstBuilder.Visitor.md#create-stream) DDL statement.

```sql
CREATE STREAM riderLocations (
    profileId VARCHAR,
    latitude DOUBLE,
    longitude DOUBLE)
  WITH (
    kafka_topic='locations',
    value_format='json',
    partitions=1);
```

## Produce JSON Record

```text
echo '{"profileId":0, "latitude":10.5, "longitude": 20.1}' | kcat -P -b :9092 -t locations
```

## Execute Pull Query

Issue a pull query.

```text
ksql> SELECT * FROM riderLocations;
+------------------------------------------------+------------------------------------------------+------------------------------------------------+
|PROFILEID                                       |LATITUDE                                        |LONGITUDE                                       |
+------------------------------------------------+------------------------------------------------+------------------------------------------------+
|0                                               |10.5                                            |20.1                                            |
Query Completed
Query terminated
```

## Explain Query

```text
ksql> EXPLAIN SELECT * FROM riderLocations;

ID                   : transient_RIDERLOCATIONS_5838976163364029274
Query Type           : PUSH
SQL                  : SELECT * FROM riderLocations;

 Field     | Type
-----------------------------
 PROFILEID | VARCHAR(STRING)
 LATITUDE  | DOUBLE
 LONGITUDE | DOUBLE
-----------------------------

Sources that this query reads from:
-----------------------------------
RIDERLOCATIONS

For source description please run: DESCRIBE [EXTENDED] <SourceId>

Execution plan
--------------
 > [ PROJECT ] | Schema: PROFILEID STRING, LATITUDE DOUBLE, LONGITUDE DOUBLE | Logger: transient_RIDERLOCATIONS_5838976163364029274.Project
   > [ SOURCE ] | Schema: PROFILEID STRING, LATITUDE DOUBLE, LONGITUDE DOUBLE, ROWTIME BIGINT, ROWPARTITION INTEGER, ROWOFFSET BIGINT | Logger: transient_RIDERLOCATIONS_5838976163364029274.KsqlTopic.Source


Processing topology
-------------------
Topologies:
   Sub-topology: 0
    Source: KSTREAM-SOURCE-0000000000 (topics: [locations])
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

## Describe Stream

```text
ksql> DESCRIBE riderlocations;

Name                 : RIDERLOCATIONS
 Field     | Type
-----------------------------
 PROFILEID | VARCHAR(STRING)
 LATITUDE  | DOUBLE
 LONGITUDE | DOUBLE
-----------------------------
For runtime statistics and query details run: DESCRIBE <Stream,Table> EXTENDED;
```

```text
ksql> DESCRIBE riderlocations EXTENDED;

Name                 : RIDERLOCATIONS
Type                 : STREAM
Timestamp field      : Not set - using <ROWTIME>
Key format           : KAFKA
Value format         : JSON
Kafka topic          : locations (partitions: 1, replication: 1)
Statement            : CREATE STREAM RIDERLOCATIONS (PROFILEID STRING, LATITUDE DOUBLE, LONGITUDE DOUBLE) WITH (KAFKA_TOPIC='locations', KEY_FORMAT='KAFKA', PARTITIONS=1, VALUE_FORMAT='JSON');

 Field     | Type
-----------------------------
 PROFILEID | VARCHAR(STRING)
 LATITUDE  | DOUBLE
 LONGITUDE | DOUBLE
-----------------------------

Local runtime statistics
------------------------


(Statistics of the local KSQL server interaction with the Kafka topic locations)
```

## Disable Stream Pull Queries

The above steps worked just fine because pull queries on streams are enabled by default (based on [ksql.query.pull.stream.enabled](../KsqlConfig.md#ksql.query.pull.stream.enabled)).

Let's turn it off and see the result. In the `ksql` CLI execute the following command:

```sql
SET 'ksql.query.pull.stream.enabled' = 'false';
```

This time executing the following query will inevitably lead to an exception.

=== "KSQL"

    ```sql
    SELECT * FROM riderLocations;
    ```

```text
Pull queries on streams are disabled. To create a push query on the stream, add EMIT CHANGES to the end. To enable pull queries on streams, set the ksql.query.pull.stream.enabled config to 'true'.
Statement: SELECT * FROM riderLocations;: SELECT * FROM riderLocations;
```
