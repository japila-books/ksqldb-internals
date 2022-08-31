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

`SHOW STREAMS` is parsed using [AstBuilder.Visitor](../parser/AstBuilder_Visitor.md#visitListStreams) into a [ListStreams](../parser/ListStreams.md) that is in turn executed using [ListSourceExecutor](../rest/ListSourceExecutor.md#streams).
