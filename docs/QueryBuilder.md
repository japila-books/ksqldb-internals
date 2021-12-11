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

!!! danger "Kafka Streams"
    `buildTransientQuery` is given a new `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder)) that is used to build a [RuntimeBuildContext](#buildContext) and then a `Topology` ([Kafka Streams]({{ book.kafka_streams }}/Topology)).

    That means that the Kafka Streams topology can only be created while [building the RuntimeBuildContext](#buildContext).

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

## <span id="buildQueryImplementation"> buildQueryImplementation

```java
Object buildQueryImplementation(
  ExecutionStep<?> physicalPlan,
  RuntimeBuildContext runtimeBuildContext)
```

`buildQueryImplementation` creates a [KSPlanBuilder](KSPlanBuilder.md) with the given [RuntimeBuildContext](RuntimeBuildContext.md).

In the end, `buildQueryImplementation` requests the given [physical plan](ExecutionStep.md) to [build a Kafka Streams application](ExecutionStep.md#build) (with the `KSPlanBuilder`).

`buildQueryImplementation` is used when:

* `QueryBuilder` is requested to [buildTransientQuery](#buildTransientQuery), [buildPersistentQueryInDedicatedRuntime](#buildPersistentQueryInDedicatedRuntime), [buildPersistentQueryInSharedRuntime](#buildPersistentQueryInSharedRuntime) and [getNamedTopology](#getNamedTopology)
