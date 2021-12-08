# RuntimeBuildContext

## Creating Instance

`RuntimeBuildContext` takes the following to be created:

* <span id="streamsBuilder"> StreamsBuilder
* <span id="ksqlConfig"> KsqlConfig
* <span id="serviceContext"> ServiceContext
* <span id="processingLogContext"> ProcessingLogContext
* <span id="functionRegistry"> FunctionRegistry
* <span id="applicationId"> String
* <span id="queryId"> QueryId
* <span id="keySerdeFactory"> KeySerdeFactory
* <span id="valueSerdeFactory"> ValueSerdeFactory

`RuntimeBuildContext` is created using [of](#of) factory.

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

* `QueryBuilder` is requested to [build a RuntimeBuildContext](QueryBuilder.md#buildContext)
