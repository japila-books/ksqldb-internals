# QueryBuilder

## Creating Instance

`QueryBuilder` takes the following to be created:

* <span id="config"> [SessionConfig](SessionConfig.md)
* <span id="processingLogContext"> [ProcessingLogContext](processing-log/ProcessingLogContext.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="functionRegistry"> [FunctionRegistry](FunctionRegistry.md)
* [KafkaStreamsBuilder](#kafkaStreamsBuilder)
* <span id="materializationProviderBuilderFactory"> `MaterializationProviderBuilderFactory`
* <span id="streams"> [SharedKafkaStreamsRuntime](SharedKafkaStreamsRuntime.md)s
* [real Flag](#real)

`QueryBuilder` is created when:

* `QueryRegistryImpl` is [created](QueryRegistryImpl.md#queryBuilderFactory)

### <span id="real"> real Flag

`QueryBuilder` is given `real` flag when [created](#creating-instance).

The flag is used to indicate whether executed in a non-sandboxed (_real_) environment or not (_sandbox_) when:

* [buildPersistentQueryInSharedRuntime](#buildPersistentQueryInSharedRuntime) (to decide what [PersistentQueryMetadata](PersistentQueryMetadata.md) to use)
* [getKafkaStreamsInstance](#getKafkaStreamsInstance) (to decide what [SharedKafkaStreamsRuntime](SharedKafkaStreamsRuntime.md) to use)

## <span id="kafkaStreamsBuilder"> KafkaStreamsBuilder

`QueryBuilder` can be given a [KafkaStreamsBuilder](KafkaStreamsBuilder.md) when [created](#creating-instance). Unless given, `QueryBuilder` creates a [KafkaStreamsBuilderImpl](KafkaStreamsBuilderImpl.md) with the [KafkaClientSupplier](ServiceContext.md#getKafkaClientSupplier) from the given [ServiceContext](#serviceContext).

The `KafkaStreamsBuilder` is used when:

* [buildTransientQuery](#buildTransientQuery)
* [buildPersistentQueryInDedicatedRuntime](#buildPersistentQueryInDedicatedRuntime)
* [getKafkaStreamsInstance](#getKafkaStreamsInstance)

## <span id="buildTransientQuery"> Building Transient Query

```java
TransientQueryMetadata buildTransientQuery(
  String statementText,
  QueryId queryId,
  Set<SourceName> sources,
  ExecutionStep<?> physicalPlan,
  String planSummary,
  LogicalSchema schema,
  OptionalInt limit,
  Optional<WindowInfo> windowInfo,
  boolean excludeTombstones,
  QueryMetadata.Listener listener,
  StreamsBuilder streamsBuilder,
  Optional<ImmutableMap<TopicPartition, Long>> endOffsets)
```

`buildTransientQuery` requests the [SessionConfig](#config) for the [KsqlConfig](SessionConfig.md#getConfig) (with overrides applied).

!!! important "Kafka Streams"
    This is the moment where ksqlDB relies on Kafka Streams (when [building a QueryImplementation](#buildQueryImplementation))

`buildTransientQuery` builds the following:

* [Application ID](QueryApplicationId.md#build) (with `persistent` flag disabled)
* [RuntimeBuildContext](#buildContext)
* [Configuration properties](#buildStreamsProperties)
* [QueryImplementation](#buildQueryImplementation)
* [TransientQueryQueue](#buildTransientQueryQueue)

`buildTransientQuery` requests the given `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder)) to build a `Topology` ([Kafka Streams]({{ book.kafka_streams }}/Topology)).

`buildTransientQuery` determines a `ResultType` (based on the `QueryImplementation` and the optional `windowInfo`):

* `WINDOWED_TABLE` for a `KTableHolder` with the `windowInfo` specified
* `TABLE` for a `KTableHolder` with no `windowInfo` specified
* `STREAM` for all other cases

In the end, `buildTransientQuery` creates a [TransientQueryMetadata](TransientQueryMetadata.md).

---

`buildTransientQuery` is used when:

* `QueryRegistryImpl` is requested to [createTransientQuery](QueryRegistryImpl.md#createTransientQuery) and [createStreamPullQuery](QueryRegistryImpl.md#createStreamPullQuery)

## <span id="buildContext"> Building RuntimeBuildContext

```java
RuntimeBuildContext buildContext(
  String applicationId,
  QueryId queryId,
  StreamsBuilder streamsBuilder)
```

`buildContext` [creates a RuntimeBuildContext](RuntimeBuildContext.md#of).

---

`buildContext` is used when:

* `QueryBuilder` is requested to [buildTransientQuery](#buildTransientQuery), [buildPersistentQueryInDedicatedRuntime](#buildPersistentQueryInDedicatedRuntime), [buildPersistentQueryInSharedRuntime](#buildPersistentQueryInSharedRuntime) and [getNamedTopology](#getNamedTopology)

## <span id="buildQueryImplementation"> Building Query Implementation

```java
Object buildQueryImplementation(
  ExecutionStep<?> physicalPlan,
  RuntimeBuildContext runtimeBuildContext)
```

!!! warning "Kafka Streams"
    This is the moment in a ksqlDB query's life cycle when the [physical plan](ExecutionStep.md) is converted into a Kafka Streams application.

`buildQueryImplementation` creates a [KSPlanBuilder](KSPlanBuilder.md) with the given [RuntimeBuildContext](RuntimeBuildContext.md).

In the end, `buildQueryImplementation` requests the given [ExecutionStep physical plan](ExecutionStep.md) to [build a Kafka Streams application](ExecutionStep.md#build) (with the `KSPlanBuilder`).

---

`buildQueryImplementation` is used when `QueryBuilder` is requested to build the following:

* [Transient query](#buildTransientQuery)
* [Persistent query in a dedicated](#buildPersistentQueryInDedicatedRuntime) or [shared](#buildPersistentQueryInSharedRuntime) runtime
* [getNamedTopology](#getNamedTopology)

## <span id="buildPersistentQueryInDedicatedRuntime"> Building Persistent Query (Dedicated Runtime)

```java
PersistentQueryMetadata buildPersistentQueryInDedicatedRuntime(
  KsqlConfig ksqlConfig,
  KsqlConstants.PersistentQueryType persistentQueryType,
  String statementText,
  QueryId queryId,
  Optional<DataSource> sinkDataSource,
  Set<DataSource> sources,
  ExecutionStep<?> physicalPlan,
  String planSummary,
  QueryMetadata.Listener listener,
  Supplier<List<PersistentQueryMetadata>> allPersistentQueries,
  StreamsBuilder streamsBuilder,
  MetricCollectors metricCollectors)
```

`buildPersistentQueryInDedicatedRuntime` [builds an application ID](QueryApplicationId.md#build) (with the `persistent` flag enabled).

`buildPersistentQueryInDedicatedRuntime` [builds streams properties](#buildStreamsProperties).

`buildPersistentQueryInDedicatedRuntime` builds a physical schema.

`buildPersistentQueryInDedicatedRuntime` [buildContext](#buildContext) (with the application ID, the given `QueryId` and `StreamsBuilder`).

!!! important "Kafka Streams"
    This is the moment where ksqlDB relies on Kafka Streams (when [building a QueryImplementation](#buildQueryImplementation))

`buildPersistentQueryInDedicatedRuntime` [buildQueryImplementation](#buildQueryImplementation) (with the given [ExecutionStep physical plan](ExecutionStep.md) and the [RuntimeBuildContext](RuntimeBuildContext.md)).

`buildPersistentQueryInDedicatedRuntime` requests the given `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder)) to build a topology (with the streams properties).

In the end, `buildPersistentQueryInDedicatedRuntime` creates a [PersistentQueryMetadataImpl](PersistentQueryMetadataImpl.md).

---

`buildPersistentQueryInDedicatedRuntime` is used when:

* `QueryRegistryImpl` is requested to [createOrReplacePersistentQuery](QueryRegistryImpl.md#createOrReplacePersistentQuery) (with no shared runtime ID or [ksql.runtime.feature.shared.enabled](KsqlConfig.md#KSQL_SHARED_RUNTIME_ENABLED) disabled)

## <span id="buildPersistentQueryInSharedRuntime"> Building Persistent Query (Shared Runtime)

```java
PersistentQueryMetadata buildPersistentQueryInSharedRuntime(
  KsqlConfig ksqlConfig,
  KsqlConstants.PersistentQueryType persistentQueryType,
  String statementText,
  QueryId queryId,
  Optional<DataSource> sinkDataSource,
  Set<DataSource> sources,
  ExecutionStep<?> physicalPlan,
  String planSummary,
  QueryMetadata.Listener listener,
  Supplier<List<PersistentQueryMetadata>> allPersistentQueries,
  String applicationId,
  MetricCollectors metricCollectors)
```

`buildPersistentQueryInSharedRuntime` [looks up the (shared) KafkaStreams instance](#getKafkaStreamsInstance) for the given `applicationId`.

`buildPersistentQueryInSharedRuntime` requests the (shared) KafkaStreams instance for the `KafkaStreams` instance to create a `NamedTopologyBuilder` (Kafka Streams) for the given `queryId`.

`buildPersistentQueryInSharedRuntime` [builds a query implementation](#buildQueryImplementation) for the given [physical plan](ExecutionStep.md).

`buildPersistentQueryInSharedRuntime` requests the `NamedTopologyBuilder` to build a `NamedTopology` (Kafka Streams).

`buildPersistentQueryInSharedRuntime`...FIXME

---

`buildPersistentQueryInSharedRuntime` is used when:

* `QueryRegistryImpl` is requested to [createOrReplacePersistentQuery](QueryRegistryImpl.md#createOrReplacePersistentQuery)

### <span id="getNamedTopology"> Finding NamedTopology

```java
NamedTopology getNamedTopology(
  SharedKafkaStreamsRuntime sharedRuntime,
  QueryId queryId,
  String applicationId,
  Map<String, Object>  queryOverrides,
  ExecutionStep<?> physicalPlan)
```

`getNamedTopology`...FIXME

### <span id="getKafkaStreamsInstance"> getKafkaStreamsInstance

```java
SharedKafkaStreamsRuntime getKafkaStreamsInstance(
  Set<SourceName> sources,
  QueryId queryID,
  MetricCollectors metricCollectors)
```

`getKafkaStreamsInstance`...FIXME

## <span id="buildStreamsProperties"> Building Streams Properties

```java
Map<String, Object> buildStreamsProperties(
  String applicationId,
  Optional<QueryId> queryId,
  MetricCollectors metricCollectors,
  KsqlConfig config,
  ProcessingLogContext processingLogContext)
```

`buildStreamsProperties` requests the given [KsqlConfig](KsqlConfig.md) for [getKsqlStreamConfigProps](KsqlConfig.md#getKsqlStreamConfigProps) for the given `applicationId`.

`buildStreamsProperties` sets `StreamsConfig.APPLICATION_ID_CONFIG` to the given `applicationId`.

`buildStreamsProperties` requests the given [ProcessingLogContext](processing-log/ProcessingLogContext.md) for [ProcessingLoggerFactory](processing-log/ProcessingLogContext.md#getLoggerFactory) to [build a ProcessingLogger](processing-log/ProcessingLoggerFactory.md#getLogger).

`buildStreamsProperties` sets `ProductionExceptionHandlerUtil.KSQL_PRODUCTION_ERROR_LOGGER` to the [ProcessingLogger](processing-log/ProcessingLogger.md).

`buildStreamsProperties`...FIXME

---

`buildStreamsProperties` is used when:

* `QueryBuilder` is requested to build a [transient query](#buildTransientQuery) and a persistent query in [dedicated](#buildPersistentQueryInDedicatedRuntime) or [shared](#buildPersistentQueryInSharedRuntime) runtime (and [getKafkaStreamsInstance](#getKafkaStreamsInstance))
* `QueryRegistryImpl` is requested to [updateStreamsPropertiesAndRestartRuntime](QueryRegistryImpl.md#updateStreamsPropertiesAndRestartRuntime) (and [updateStreamsProperties](QueryRegistryImpl.md#updateStreamsProperties))

## <span id="getUncaughtExceptionProcessingLogger"> getUncaughtExceptionProcessingLogger

```java
ProcessingLogger getUncaughtExceptionProcessingLogger(
  QueryId queryId)
```

`getUncaughtExceptionProcessingLogger`...FIXME

---

`getUncaughtExceptionProcessingLogger` is used when:

* `QueryBuilder` is requested to build persistent query in [dedicated](QueryBuilder.md#buildPersistentQueryInDedicatedRuntime) or [shared](QueryBuilder.md#buildPersistentQueryInSharedRuntime) runtime (and build a [PersistentQueryMetadata](PersistentQueryMetadata.md#getProcessingLogger))
