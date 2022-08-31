# QueryRegistry

`QueryRegistry` is an [abstraction](#contract) of [query registries](#implementations) for building and managing queries.

## Contract (Subset)

### <span id="createOrReplacePersistentQuery"> createOrReplacePersistentQuery

```java
PersistentQueryMetadata createOrReplacePersistentQuery(
  SessionConfig config,
  ServiceContext serviceContext,
  ProcessingLogContext processingLogContext,
  MetaStore metaStore,
  String statementText,
  QueryId queryId,
  Optional<DataSource> sinkDataSource,
  Set<DataSource> sources,
  ExecutionStep<?> physicalPlan,
  String planSummary,
  KsqlConstants.PersistentQueryType persistentQueryType,
  Optional<String> sharedRuntimeId)
```

See [QueryRegistryImpl.createOrReplacePersistentQuery](QueryRegistryImpl.md#createOrReplacePersistentQuery)

Used when:

* `EngineExecutor` is requested to [execute a persistent query](EngineExecutor.md#executePersistentQuery)

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

### <span id="getAllLiveQueries"> getAllLiveQueries

```java
List<QueryMetadata> getAllLiveQueries()
```

All active queries ([QueryMetadata](QueryMetadata.md))

[QueryRegistryImpl.getAllLiveQueries](QueryRegistryImpl.md#getAllLiveQueries)

Used when:

* `EngineExecutor` is requested to [executeTransientQuery](EngineExecutor.md#executeTransientQuery), [executeStreamPullQuery](EngineExecutor.md#executeStreamPullQuery), [sourceTablePlan](EngineExecutor.md#sourceTablePlan), [plan a statement](EngineExecutor.md#plan)
* `KsqlEngine` is requested to [getAllLiveQueries](KsqlEngine.md#getAllLiveQueries)
* `SandboxedExecutionContext` is requested to [getAllLiveQueries](SandboxedExecutionContext.md#getAllLiveQueries)
* `TransientQueryCleanupService` is requested to `isCorrespondingQueryTerminated`
* `QueryRegistryImpl` is requested to [close live queries](QueryRegistryImpl.md#close)

## Implementations

* [QueryRegistryImpl](QueryRegistryImpl.md)
