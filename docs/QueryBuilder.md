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

## <span id="buildTransientQuery"> buildTransientQuery

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

`buildTransientQuery` [builds an application ID](QueryApplicationId.md#build) (with `persistent` flag disabled).

`buildTransientQuery` builds a [RuntimeBuildContext](#buildContext), [buildStreamsProperties](#buildStreamsProperties), [buildQueryImplementation](#buildQueryImplementation) and [buildTransientQueryQueue](#buildTransientQueryQueue).

`buildTransientQuery` requests the given `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder)) to build a topology.

`buildTransientQuery`...FIXME

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

`buildQueryImplementation` requests the given `physicalPlan` to [build](ExecutionStep.md#build) (wiht the `PlanBuilder`).

`buildQueryImplementation` is used when:

* `QueryBuilder` is requested to [buildTransientQuery](#buildTransientQuery), [buildPersistentQueryInDedicatedRuntime](#buildPersistentQueryInDedicatedRuntime), [buildPersistentQueryInSharedRuntime](#buildPersistentQueryInSharedRuntime) and [getNamedTopology](#getNamedTopology)
