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

### <span id="createStreamPullQuery"> Creating Stream Pull Query

```java
TransientQueryMetadata createStreamPullQuery(
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
  boolean excludeTombstones,
  ImmutableMap<TopicPartition, Long> endOffsets)
```

!!! note
    `createStreamPullQuery` is [createTransientQuery](#createTransientQuery) with `endOffsets`.

See [QueryRegistryImpl.createStreamPullQuery](QueryRegistryImpl.md#createStreamPullQuery)

Used when:

* `EngineExecutor` is requested to [executeStreamPullQuery](EngineExecutor.md#executeStreamPullQuery)

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

!!! note
    `createTransientQuery` is [createStreamPullQuery](#createStreamPullQuery) with no `endOffsets`.

See [QueryRegistryImpl.createTransientQuery](QueryRegistryImpl.md#createTransientQuery)

Used when:

* `EngineExecutor` is requested to [executeTransientQuery](EngineExecutor.md#executeTransientQuery)

### <span id="getAllLiveQueries"> getAllLiveQueries

```java
List<QueryMetadata> getAllLiveQueries()
```

All active queries ([QueryMetadata](QueryMetadata.md))

See [QueryRegistryImpl.getAllLiveQueries](QueryRegistryImpl.md#getAllLiveQueries)

Used when:

* `EngineExecutor` is requested to [executeTransientQuery](EngineExecutor.md#executeTransientQuery), [executeStreamPullQuery](EngineExecutor.md#executeStreamPullQuery), [sourceTablePlan](EngineExecutor.md#sourceTablePlan), [plan a statement](EngineExecutor.md#plan)
* `KsqlEngine` is requested to [getAllLiveQueries](KsqlEngine.md#getAllLiveQueries)
* `SandboxedExecutionContext` is requested to [getAllLiveQueries](SandboxedExecutionContext.md#getAllLiveQueries)
* `TransientQueryCleanupService` is requested to `isCorrespondingQueryTerminated`
* `QueryRegistryImpl` is requested to [close live queries](QueryRegistryImpl.md#close)

### <span id="updateStreamsPropertiesAndRestartRuntime"> updateStreamsPropertiesAndRestartRuntime

```java
void updateStreamsPropertiesAndRestartRuntime(
  KsqlConfig config,
  ProcessingLogContext logContext)
```

See [QueryRegistryImpl.updateStreamsPropertiesAndRestartRuntime](QueryRegistryImpl.md#updateStreamsPropertiesAndRestartRuntime)

Used when:

* `KsqlEngine` is requested to [updateStreamsPropertiesAndRestartRuntime](KsqlEngine.md#updateStreamsPropertiesAndRestartRuntime)

## Implementations

* [QueryRegistryImpl](QueryRegistryImpl.md)
