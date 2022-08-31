# Pull Queries

**Pull Queries** are non-persistent `SELECT` queries with no `EMIT CHANGES` clause (that would turn such queries into [push queries](push-queries.md)).

Pull Queries retrieve the latest result from a source (a materialized view, a table, or a stream) instantly and as of "now".

Pull queries are printed out only in the console.

Pull queries follow a traditional request/response model. They retrieve a finite result from the ksqlDB server and terminate (like in traditional databases).

Pull queries use an eventually consistent consistency model.

!!! note
    Learn more in the official documentation of ksqlDB [here](https://docs.ksqldb.io/en/latest/developer-guide/ksqldb-reference/select-pull-query/) and [here](https://docs.ksqldb.io/en/latest/concepts/queries/#pull).

## Demo

Create an input stream.

```sql
CREATE STREAM riderLocations (profileId VARCHAR, latitude DOUBLE, longitude DOUBLE)
  WITH (
    kafka_topic='locations',
    value_format='json',
    partitions=1);
```

```text
 Message
----------------
 Stream created
----------------
```

```text
echo '{"profileId":0, "latitude":10.5, "longitude": 20.1}' | kcat -P -b :9092 -t locations
```

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

```text
ksql> explain SELECT * FROM riderLocations;

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

```text
ksql> DESCRIBE RIDERLOCATIONS EXTENDED;

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
ksql> DESCRIBE RIDERLOCATIONS EXTENDED;

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
