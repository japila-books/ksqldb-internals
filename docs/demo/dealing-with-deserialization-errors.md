# Demo: Dealing with Deserialization Errors

This demo is a follow-up to [Demo: Table isn't queryable](table-isnt-queryable.md) where you can learn how to deal with deserialization errors.

In [Solution: Create Source Table](table-isnt-queryable.md#solution-create-source-table), the demo uses `CREATE SOURCE TABLE` with `id STRING PRIMARY KEY`.

The issue this demo is presenting is what happens when the table uses non-`STRING` type for the primary key and the keys simply do not match type-wise.

!!! note
    It turns out that `kcat` and `kafka-console-producer` tools can send out records with keys as `STRING`s only (!)

## Logging

Enable [io.confluent.ksql.errors.LogMetricAndContinueExceptionHandler](../metrics/LogMetricAndContinueExceptionHandler.md#logging) logger to see what happens under the covers.

## (Re)Create Source Table

=== "KSQL"

    ```sql
    DROP TABLE my_source_table IF EXISTS;
    CREATE SOURCE TABLE my_source_table (
        id INTEGER PRIMARY KEY,
        name STRING)
    WITH (
        KAFKA_TOPIC='my_topic',
        VALUE_FORMAT='json');
    ```

Depending on what records are available in `my_topic`, you may already see the following exceptions:

<style>
  code {
    white-space : pre-wrap !important;
    word-break: break-word;
  }
</style>

```text
ERROR {"type":0,"deserializationError":{"target":"key","errorMessage":"Error deserializing KAFKA message from topic: my_topic","recordB64":null,"cause":["Size of data received by IntegerDeserializer is not 4"],"topic":"my_topic"},"recordProcessingError":null,"productionError":null,"serializationError":null,"kafkaStreamsThreadError":null} (processing.CST_MY_SOURCE_TABLE_9.KsqlTopic.Source.deserializer:44)
```

```text
DEBUG Exception caught during Deserialization, taskId: 0_0, topic: my_topic, partition: 0, offset: 1 (io.confluent.ksql.errors.LogMetricAndContinueExceptionHandler:40)
org.apache.kafka.common.errors.SerializationException: Error deserializing KAFKA message from topic: my_topic
	at io.confluent.ksql.serde.kafka.KafkaSerdeFactory$RowDeserializer.deserialize(KafkaSerdeFactory.java:147)
	at io.confluent.ksql.serde.kafka.KafkaSerdeFactory$RowDeserializer.deserialize(KafkaSerdeFactory.java:129)
	at io.confluent.ksql.serde.GenericDeserializer.deserialize(GenericDeserializer.java:59)
	at io.confluent.ksql.logging.processing.LoggingDeserializer.tryDeserialize(LoggingDeserializer.java:61)
	at io.confluent.ksql.logging.processing.LoggingDeserializer.deserialize(LoggingDeserializer.java:48)
	at org.apache.kafka.common.serialization.Deserializer.deserialize(Deserializer.java:60)
	at org.apache.kafka.streams.processor.internals.SourceNode.deserializeKey(SourceNode.java:54)
	at org.apache.kafka.streams.processor.internals.RecordDeserializer.deserialize(RecordDeserializer.java:65)
	at org.apache.kafka.streams.processor.internals.RecordQueue.updateHead(RecordQueue.java:208)
	at org.apache.kafka.streams.processor.internals.RecordQueue.addRawRecords(RecordQueue.java:131)
	at org.apache.kafka.streams.processor.internals.PartitionGroup.addRawRecords(PartitionGroup.java:315)
	at org.apache.kafka.streams.processor.internals.StreamTask.addRecords(StreamTask.java:987)
	at org.apache.kafka.streams.processor.internals.TaskManager.addRecordsToTasks(TaskManager.java:1065)
	at org.apache.kafka.streams.processor.internals.StreamThread.pollPhase(StreamThread.java:966)
	at org.apache.kafka.streams.processor.internals.StreamThread.runOnce(StreamThread.java:750)
	at org.apache.kafka.streams.processor.internals.StreamThread.runLoop(StreamThread.java:594)
	at org.apache.kafka.streams.processor.internals.StreamThread.run(StreamThread.java:556)
Caused by: org.apache.kafka.common.errors.SerializationException: Size of data received by IntegerDeserializer is not 4
	at org.apache.kafka.common.serialization.IntegerDeserializer.deserialize(IntegerDeserializer.java:26)
	at org.apache.kafka.common.serialization.IntegerDeserializer.deserialize(IntegerDeserializer.java:21)
	at io.confluent.ksql.serde.kafka.KafkaSerdeFactory$RowDeserializer.deserialize(KafkaSerdeFactory.java:140)
	... 16 more
```

If not, use the following `kcat` command to trigger them:

```shell
echo '{"id": 1, "name": "ONE"}' | kcat -P -b :9092 -t my_topic -k 1
```

This is because the records use keys that are `STRING`s but the table expects `INTEGER`s.

## Runtime Statistics

Not only does the logs show the exception, but there are metrics that you can monitor and be notified about this issue:

* [consumer-failed-messages](../metrics/StreamsErrorCollector.md#consumer-failed-messages)
* [consumer-failed-messages-per-sec](../metrics/StreamsErrorCollector.md#consumer-failed-messages-per-sec)

=== "KSQL"

    ```sql
    LIST TABLES;
    ```

```text
 Table Name      | Kafka Topic | Key Format | Value Format | Windowed
----------------------------------------------------------------------
 MY_SOURCE_TABLE | my_topic    | KAFKA      | JSON         | false
 MY_TABLE        | my_topic    | KAFKA      | JSON         | false
----------------------------------------------------------------------
```

Use `DESCRIBE [table] EXTENDED` for runtime statistics and query details.

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
Statement            : CREATE SOURCE TABLE MY_SOURCE_TABLE (ID INTEGER PRIMARY KEY, NAME STRING) WITH (KAFKA_TOPIC='my_topic', KEY_FORMAT='KAFKA', VALUE_FORMAT='JSON');

 Field | Type
----------------------------------------
 ID    | INTEGER          (primary key)
 NAME  | VARCHAR(STRING)
----------------------------------------

Queries that read from this TABLE
-----------------------------------
CST_MY_SOURCE_TABLE_9 (RUNNING) : CREATE SOURCE TABLE MY_SOURCE_TABLE (ID INTEGER PRIMARY KEY, NAME STRING) WITH (KAFKA_TOPIC='my_topic', KEY_FORMAT='KAFKA', VALUE_FORMAT='JSON');

For query topology and execution plan please run: EXPLAIN <QueryId>

Runtime statistics by host
-------------------------
 Host           | Metric                           | Value      | Last Message
-------------------------------------------------------------------------------------------
 localhost:8088 | consumer-failed-messages         |          2 | 2022-09-21T07:20:55.383Z
 localhost:8088 | consumer-failed-messages-per-sec |          0 | 2022-09-21T07:20:55.383Z
 localhost:8088 | consumer-messages-per-sec        |          0 | 2022-09-21T07:20:55.37Z
 localhost:8088 | consumer-total-bytes             |         50 | 2022-09-21T07:20:55.37Z
 localhost:8088 | consumer-total-messages          |          2 | 2022-09-21T07:20:55.37Z
-------------------------------------------------------------------------------------------
(Statistics of the local KSQL server interaction with the Kafka topic my_topic)
```
