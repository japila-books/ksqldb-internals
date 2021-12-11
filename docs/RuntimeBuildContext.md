# RuntimeBuildContext

## Creating Instance

`RuntimeBuildContext` takes the following to be created:

* [StreamsBuilder](#streamsBuilder)
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="processingLogContext"> `ProcessingLogContext`
* <span id="functionRegistry"> `FunctionRegistry`
* <span id="applicationId"> Application ID
* <span id="queryId"> `QueryId`
* <span id="keySerdeFactory"> `KeySerdeFactory`
* <span id="valueSerdeFactory"> `ValueSerdeFactory`

`RuntimeBuildContext` is created using [of](#of) factory.

## <span id="streamsBuilder"><span id="getStreamsBuilder"> StreamsBuilder

`RuntimeBuildContext` is given a `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder)) when [created](#creating-instance).

The `StreamsBuilder` is used when:

* `SourceBuilder` is requested to [buildKTable](SourceBuilder.md#buildKTable)
* `SourceBuilderV1` is requested to [buildKTable](SourceBuilderV1.md#buildKTable) and [buildKStream](SourceBuilderV1.md#buildKStream)

## <span id="of"> Creating RuntimeBuildContext

```java
RuntimeBuildContext of(
  final StreamsBuilder streamsBuilder,
  final KsqlConfig ksqlConfig,
  final ServiceContext serviceContext,
  final ProcessingLogContext processingLogContext,
  final FunctionRegistry functionRegistry,
  final String applicationId,
  final QueryId queryId)
```

`of` creates a [RuntimeBuildContext](#creating-instance) (with a `GenericKeySerDe` and `GenericRowSerDe`).

`of` is used when:

* `QueryBuilder` is requested for a [RuntimeBuildContext](QueryBuilder.md#buildContext)
