# QueryRegistryImpl

## <span id="createTransientQuery"> Creating Transient Query

```java
TransientQueryMetadata createTransientQuery(
  SessionConfig config,
  ServiceContext serviceContext,
  ProcessingLogContext processingLogContext,
  MetaStore metaStore,
  String statementText,
  QueryId queryId,
  Set<SourceName> sources,
  ExecutionStep<?> physicalPlan,
  String planSummary,
  LogicalSchema schema,
  OptionalInt limit,
  Optional<WindowInfo> windowInfo,
  boolean excludeTombstones)
```

`createTransientQuery` requests the [QueryBuilderFactory](#queryBuilderFactory) for a [QueryBuilder](QueryBuilderFactory.md#create).

`createTransientQuery` requests the `QueryBuilder` to [build a transient query](QueryBuilder.md#buildTransientQuery) (with a new `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder)) that gives a [TransientQueryMetadata](TransientQueryMetadata.md)).

`createTransientQuery` requests the `TransientQueryMetadata` to [initialize](QueryMetadataImpl.md#initialize).

`createTransientQuery` [registerTransientQuery](#registerTransientQuery) and returns the `TransientQueryMetadata`.

---

`createTransientQuery` is part of the [QueryRegistry](QueryRegistry.md#createTransientQuery) abstraction.
