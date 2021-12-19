# SourceBuilder

`SourceBuilder` is a [SourceBuilderBase](SourceBuilderBase.md).

## <span id="instance"> SourceBuilder Instance

`SourceBuilder` defines `instance` static value with an instance of `SourceBuilder`.

The `instance` is used when:

* `KSPlanBuilder` is requested to [visitTableSource](KSPlanBuilder.md#visitTableSource)

## <span id="buildKTable"> Building KTable

```java
KTable<K, GenericRow> buildKTable(
  SourceStep<?> streamSource,
  RuntimeBuildContext buildContext,
  Consumed<K, GenericRow> consumed,
  Function<K, Collection<?>> keyGenerator,
  Materialized<K, GenericRow, KeyValueStore<Bytes, byte[]>> materialized,
  Serde<GenericRow> valueSerde,
  String stateStoreName,
  PlanInfo planInfo)
```

`buildKTable`...FIXME

`buildKTable` is part of the [SourceBuilderBase](SourceBuilderBase.md#buildKTable) abstraction.
