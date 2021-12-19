# SourceBuilderBase

`SourceBuilderBase` is an [abstraction](#contract) of [source builders](#implementations) (that `KSPlanBuilder` uses when [visitTableSource](KSPlanBuilder.md#visitTableSource)).

## Contract

### <span id="buildKTable"> Building KTable

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

Builds a `KTable` ([Kafka Streams]({{ book.kafka_streams }}/kstream/KTable))

Used when:

* `SourceBuilderBase` is requested to [buildTable](#buildTable)
* `SourceBuilderV1` is requested to [buildWindowedTable](SourceBuilderV1.md#buildWindowedTable)

### <span id="buildTableMaterialized"> Building Table Materialized

```java
Materialized<GenericKey, GenericRow, KeyValueStore<Bytes, byte[]>>
buildTableMaterialized(
  SourceStep<KTableHolder<GenericKey>> source,
  RuntimeBuildContext buildContext,
  MaterializedFactory materializedFactory,
  Serde<GenericKey> keySerde,
  Serde<GenericRow> valueSerde,
  String stateStoreName)
```

Builds a `Materialized` ([Kafka Streams]({{ book.kafka_streams }}/kstream/Materialized))

Used when:

* `SourceBuilderBase` is requested to [buildTable](#buildTable)

## Implementations

* [SourceBuilder](SourceBuilder.md)
* [SourceBuilderV1](SourceBuilderV1.md)

## <span id="buildTable"> buildTable

```java
KTableHolder<GenericKey> buildTable(
  RuntimeBuildContext buildContext,
  SourceStep<KTableHolder<GenericKey>> source,
  ConsumedFactory consumedFactory,
  MaterializedFactory materializedFactory,
  PlanInfo planInfo)
```

`buildTable` gets a [PhysicalSchema](#getPhysicalSchema), a [ValueSerde](#getValueSerde) and a [KeySerde](#getKeySerde) (`Serde<GenericKey>`s).

`buildTable` [buildSourceConsumed](#buildSourceConsumed) (with `AutoOffsetReset.EARLIEST` offset reset).

`buildTable` [buildTableMaterialized](#buildTableMaterialized) and [buildKTable](#buildKTable) (a `KTable<GenericKey, GenericRow>`).

In the end, `buildTable` creates a `KTableHolder` (with the `KTable`).

`buildTable` is used when:

* `KSPlanBuilder` is requested to [visitTableSource](KSPlanBuilder.md#visitTableSource)
