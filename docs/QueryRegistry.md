# QueryRegistry

`QueryRegistry` is an [abstraction](#contract) of [query registries](#implementations) for building and managing queries.

## Contract (Subset)

### <span id="createTransientQuery"> createTransientQuery

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

[QueryRegistryImpl.createTransientQuery](QueryRegistryImpl.md#createTransientQuery)

Used when:

* `EngineExecutor` is requested to [executeTransientQuery](EngineExecutor.md#executeTransientQuery)

## Implementations

* [QueryRegistryImpl](QueryRegistryImpl.md)
