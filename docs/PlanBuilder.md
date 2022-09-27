# PlanBuilder

`PlanBuilder` is an [abstraction](#contract) of [query plan builders](#implementations) (for `QueryBuilder` to [build a query implementation](QueryBuilder.md#buildQueryImplementation) from an [execution plan](ExecutionStep.md#build)).

## Contract (Subset)

### <span id="visitStreamSelect"> Visiting StreamSelect

```java
KStreamHolder<K> visitStreamSelect(
  StreamSelect<K> streamSelect,
  PlanInfo planInfo)
```

Visits a [StreamSelect](StreamSelect.md)

See [KSPlanBuilder](KSPlanBuilder.md#visitStreamSelect)

Used when:

* `StreamSelect` is requested to [build a KStreamHolder](StreamSelect.md#build)

### <span id="visitStreamSelectKey"> Visiting StreamSelectKey

```java
KStreamHolder<GenericKey> visitStreamSelectKey(
  StreamSelectKeyV1 streamSelectKey,
  PlanInfo planInfo)
```

Visits a [StreamSelectKeyV1](StreamSelectKeyV1.md)

See [KSPlanBuilder](KSPlanBuilder.md#visitStreamSelectKey)

Used when:

* `StreamSelectKeyV1` is requested to [build a KStreamHolder](StreamSelectKeyV1.md#build)

### <span id="visitStreamSource"> Visiting StreamSource

```java
KStreamHolder<GenericKey> visitStreamSource(
  StreamSource streamSource,
  PlanInfo planInfo)
```

Visits a [StreamSource](StreamSource.md)

See [KSPlanBuilder](KSPlanBuilder.md#visitStreamSource)

Used when:

* `StreamSource` is requested to [build a KStreamHolder](StreamSource.md#build)

### <span id="visitTableSelect"> Visiting TableSelect

```java
<K> KTableHolder<K> visitTableSelect(
  TableSelect<K> tableSelect,
  PlanInfo planInfo)
```

Visits a [TableSelect](TableSelect.md)

See [KSPlanBuilder](KSPlanBuilder.md#visitTableSelect)

Used when:

* `TableSelect` is requested to [build a KTableHolder](TableSelect.md#build)

### <span id="visitTableSource"> Visiting TableSource

```java
KTableHolder<GenericKey> visitTableSource(
  TableSource tableSource,
  PlanInfo planInfo)
KTableHolder<GenericKey> visitTableSource(
  TableSourceV1 tableSourceV1,
  PlanInfo planInfo)  // (1)!
```

1. legacy

See [KSPlanBuilder](KSPlanBuilder.md#visitTableSource)

Used when:

* `TableSource` is requested to [build a KTableHolder](TableSource.md#build)
* `TableSourceV1` is requested to build a `KTableHolder`

## Implementations

* [KSPlanBuilder](KSPlanBuilder.md)
