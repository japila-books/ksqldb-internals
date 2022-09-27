# RuntimeBuildContext

## Creating Instance

`RuntimeBuildContext` takes the following to be created:

* [StreamsBuilder](#streamsBuilder)
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="processingLogContext"> [ProcessingLogContext](processing-log/ProcessingLogContext.md)
* <span id="functionRegistry"> [FunctionRegistry](FunctionRegistry.md)
* <span id="applicationId"> Application ID
* <span id="queryId"> Query ID
* <span id="keySerdeFactory"> [KeySerdeFactory](formats/KeySerdeFactory.md)
* <span id="valueSerdeFactory"> [ValueSerdeFactory](formats/ValueSerdeFactory.md)

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

`of` creates a [RuntimeBuildContext](#creating-instance) with the arguments and the following:

* [GenericKeySerDe](formats/GenericKeySerDe.md) for the [KeySerdeFactory](#keySerdeFactory)
* [GenericRowSerDe](formats/GenericRowSerDe.md) for the [ValueSerdeFactory](#valueSerdeFactory)

---

`of` is used when:

* `QueryBuilder` is requested for a [RuntimeBuildContext](QueryBuilder.md#buildContext)

## <span id="getProcessingLogger"> ProcessingLogger

```java
ProcessingLogger getProcessingLogger(
  QueryContext queryContext)
```

`getProcessingLogger`...FIXME

## <span id="buildValueSerde"> Building Value Serde

```java
Serde<GenericRow> buildValueSerde(
  FormatInfo format,
  PhysicalSchema schema,
  QueryContext queryContext)
```

`buildValueSerde` [builds a query logger prefix](QueryLoggerUtil.md#queryLoggerName) for the [Query ID](#queryId) (and the given `QueryContext`).

`buildValueSerde` requests the [QuerySchemas](#schemas) to `trackValueSerdeCreation`.

In the end, `buildValueSerde` requests the [ValueSerdeFactory](#valueSerdeFactory) to [create a value Serde](formats/ValueSerdeFactory.md#create).
