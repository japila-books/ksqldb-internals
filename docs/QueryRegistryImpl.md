# QueryRegistryImpl

## <span id="createTransientQuery"> createTransientQuery

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

`createTransientQuery` requests the [QueryBuilderFactory](#queryBuilderFactory) to [create a QueryBuilder](QueryBuilderFactory.md#create).

`createTransientQuery` requests the `QueryBuilder` to [buildTransientQuery](QueryBuilder.md#buildTransientQuery) (with a new `StreamsBuilder`) that gives a [TransientQueryMetadata](TransientQueryMetadata.md) back.

`createTransientQuery` requests the `TransientQueryMetadata` to [initialize](QueryMetadataImpl.md#initialize).

`createTransientQuery` [registerTransientQuery](#registerTransientQuery) and returns the `TransientQueryMetadata`.

---

`createTransientQuery` is part of the [QueryRegistry](QueryRegistry.md#createTransientQuery) abstraction.
