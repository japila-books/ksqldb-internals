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

## <span id="visitStreamSelect"> visitStreamSelect

```java
KStreamHolder<K> visitStreamSelect(
  StreamSelect<K> streamSelect,
  PlanInfo planInfo)
```

`visitStreamSelect` is part of the [PlanBuilder](PlanBuilder.md#visitStreamSelect) abstraction.

---

`visitStreamSelect` requests the given [StreamSelect](StreamSelect.md) for the [ExecutionStep of the source](StreamSelect.md#getSource) to [build](ExecutionStep.md#build).

In the end, `visitStreamSelect` [builds a KStreamHolder](StreamSelectBuilder.md#build).

## <span id="visitStreamSource"> visitStreamSource

```java
KStreamHolder<GenericKey> visitStreamSource(
  StreamSource streamSource,
  PlanInfo planInfo) // (1)!
```

`visitStreamSource` is part of the [PlanBuilder](PlanBuilder.md#visitStreamSource) abstraction.

---

1. The given `PlanInfo` is not used.

`visitStreamSource` uses the [SourceBuilderV1](SourceBuilderV1.md#instance) to [build a KStream](SourceBuilderV1.md#buildStream) (for the [RuntimeBuildContext](#buildContext), the given [StreamSource](StreamSource.md) and the `ConsumedFactory` from the [StreamsFactories](#streamsFactories)).

## <span id="visitTableSource"> visitTableSource

```java
KTableHolder<GenericKey> visitTableSource(
  TableSource tableSource,
  PlanInfo planInfo)
KTableHolder<GenericKey> visitTableSource(
  TableSourceV1 tableSourceV1,
  PlanInfo planInfo)
```

`visitTableSource` is part of the [PlanBuilder](PlanBuilder.md#visitTableSource) abstraction.

---

`visitTableSource` requests the [SourceBuilder](SourceBuilder.md#instance) or [SourceBuilderV1](SourceBuilderV1.md#instance) to [buildTable](SourceBuilderBase.md#buildTable).
