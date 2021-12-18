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

## <span id="createOrReplacePersistentQuery"> Creating or Replacing Persistent Query

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

`createOrReplacePersistentQuery` requests the [QueryBuilderFactory](#queryBuilderFactory) for a [QueryBuilder](QueryBuilderFactory.md#create) to build a persistent query in [shared](QueryBuilder.md#buildPersistentQueryInSharedRuntime) or [dedicated](QueryBuilder.md#buildPersistentQueryInDedicatedRuntime) runtime based on the given `sharedRuntimeId` (available or not, respectively).

In the end, `createOrReplacePersistentQuery` [registers the persistent query](#registerPersistentQuery).

---

`createOrReplacePersistentQuery` is part of the [QueryRegistry](QueryRegistry.md#createOrReplacePersistentQuery) abstraction.

### <span id="registerPersistentQuery"> registerPersistentQuery

```java
void registerPersistentQuery(
  ServiceContext serviceContext,
  MetaStore metaStore,
  PersistentQueryMetadata persistentQuery)
```

`registerPersistentQuery` takes the `QueryId` from the given `PersistentQueryMetadata`.

`registerPersistentQuery` requests the given `PersistentQueryMetadata` to [initialize](QueryMetadata.md#initialize) when this is a new query (a new `QueryId`) or the old query is not sandboxed.

`registerPersistentQuery` adds the `QueryId` with the `PersistentQueryMetadata` to the [persistentQueries](#persistentQueries) registry.

`registerPersistentQuery` registers the persistent query based on the [type](PersistentQueryMetadata.md#getPersistentQueryType):

* For `CREATE_SOURCE`, the single source name with the query ID in the [createAsQueries](#createAsQueries) registry

* For `CREATE_AS`, the sink name with the query ID in the [createAsQueries](#createAsQueries) registry

* For `INSERT`, all the sink and source names with the query ID in the [insertQueries](#insertQueries) registry

`registerPersistentQuery` adds the `QueryId` with the `PersistentQueryMetadata` to the [allLiveQueries](#allLiveQueries) registry.

In the end, `registerPersistentQuery` [notifies event listeners](#notifyCreate).

## <span id="notifyCreate"> Notifying QueryEventListeners about Create Queries

```java
void notifyCreate(
  ServiceContext serviceContext,
  MetaStore metaStore,
  QueryMetadata queryMetadata)
```

`notifyCreate` requests the [QueryEventListeners](#eventListeners) to [onCreate](QueryEventListener.md#onCreate)

`notifyCreate` is used when:

* `QueryRegistryImpl` is requested to [create a StreamPullQuery](#createStreamPullQuery) and register [persistent](#registerPersistentQuery) or [transient](#registerTransientQuery) queries
