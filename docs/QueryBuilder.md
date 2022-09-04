# QueryBuilder

## Creating Instance

`QueryBuilder` takes the following to be created:

* <span id="config"> `SessionConfig`
* <span id="processingLogContext"> `ProcessingLogContext`
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="functionRegistry"> `FunctionRegistry`
* [KafkaStreamsBuilder](#kafkaStreamsBuilder)
* <span id="materializationProviderBuilderFactory"> `MaterializationProviderBuilderFactory`
* <span id="streams"> [SharedKafkaStreamsRuntime](SharedKafkaStreamsRuntime.md)s
* <span id="real"> `real` flag

`QueryBuilder` is created when:

* `QueryRegistryImpl` is [created](QueryRegistryImpl.md#queryBuilderFactory)

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

In the end, `buildQueryImplementation` requests the given [physical plan](ExecutionStep.md) to [build a Kafka Streams application](ExecutionStep.md#build) (with the `KSPlanBuilder`).

`buildQueryImplementation` is used when `QueryBuilder` is requested to build the following:

* [Transient query](#buildTransientQuery)
* [Persistent query in a dedicated](#buildPersistentQueryInDedicatedRuntime) or [shared](#buildPersistentQueryInSharedRuntime) runtime
* [getNamedTopology](#getNamedTopology)

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

`buildPersistentQueryInDedicatedRuntime` [buildStreamsProperties](#buildStreamsProperties).

`buildPersistentQueryInDedicatedRuntime`...FIXME

`buildPersistentQueryInDedicatedRuntime` is used when:

* `QueryRegistryImpl` is requested to [createOrReplacePersistentQuery](QueryRegistryImpl.md#createOrReplacePersistentQuery) (with a shared runtime ID)
