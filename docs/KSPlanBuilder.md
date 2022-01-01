# KSPlanBuilder

`KSPlanBuilder` is a [PlanBuilder](PlanBuilder.md) that builds a Kafka Streams application (from an [execution plan](ExecutionStep.md#build)).

## Creating Instance

`KSPlanBuilder` takes the following to be created:

* <span id="buildContext"> [RuntimeBuildContext](RuntimeBuildContext.md)
* <span id="sqlPredicateFactory"> `SqlPredicateFactory`
* <span id="aggregateParamFactory"> `AggregateParamsFactory`
* <span id="streamsFactories"> `StreamsFactories`

`KSPlanBuilder` is created when:

* `QueryBuilder` is requested to [build a query implementation](QueryBuilder.md#buildQueryImplementation)

## <span id="visitStreamSource"> visitStreamSource

```java
KStreamHolder<GenericKey> visitStreamSource(
  StreamSource streamSource,
  PlanInfo planInfo) // (1)!
```

1. The given `PlanInfo` is not used.

`visitStreamSource` uses the [SourceBuilderV1](SourceBuilderV1.md#instance) to [build a KStream](SourceBuilderV1.md#buildStream) (for the [RuntimeBuildContext](#buildContext), the given [StreamSource](StreamSource.md) and the `ConsumedFactory` from the [StreamsFactories](#streamsFactories)).

`visitStreamSource` is part of the [PlanBuilder](PlanBuilder.md#visitStreamSource) abstraction.

## <span id="visitTableSource"> visitTableSource

```java
KTableHolder<GenericKey> visitTableSource(
  TableSource tableSource,
  PlanInfo planInfo)
KTableHolder<GenericKey> visitTableSource(
  TableSourceV1 tableSourceV1,
  PlanInfo planInfo)
```

`visitTableSource` requests the [SourceBuilder](SourceBuilder.md#instance) or [SourceBuilderV1](SourceBuilderV1.md#instance) to [buildTable](SourceBuilderBase.md#buildTable).

`visitTableSource` is part of the [PlanBuilder](PlanBuilder.md#visitTableSource) abstraction.
