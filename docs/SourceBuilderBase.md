# SourceBuilderBase

`SourceBuilderBase` is an [abstraction](#contract) of [source builders](#implementations) (that `KSPlanBuilder` uses when [visiting a TableSource](KSPlanBuilder.md#visitTableSource)).

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

## <span id="buildTable"> Building KTableHolder

```java
KTableHolder<GenericKey> buildTable(
  RuntimeBuildContext buildContext,
  SourceStep<KTableHolder<GenericKey>> source,
  ConsumedFactory consumedFactory,
  MaterializedFactory materializedFactory,
  PlanInfo planInfo)
```

`buildTable` gets a [PhysicalSchema](#getPhysicalSchema), a [ValueSerde](#getValueSerde) and a [KeySerde](#getKeySerde) (`Serde<GenericKey>`s) of the given [SourceStep](SourceStep.md).

`buildTable` [builds](#buildSourceConsumed) a `Consumed<GenericKey, GenericRow>` ([Kafka Streams]({{ book.kafka_streams }}/kstream/Consumed)) for the given [SourceStep](SourceStep.md) (with `AutoOffsetReset.EARLIEST` offset reset).

`buildTable` [tableChangeLogOpName](#tableChangeLogOpName).

`buildTable` [builds](#buildTableMaterialized) a `Materialized<GenericKey, GenericRow, KeyValueStore<Bytes, byte[]>>` ([Kafka Streams]({{ book.kafka_streams }}/kstream/Materialized)).

`buildTable` [builds](#buildKTable) a `KTable<GenericKey, GenericRow>` ([Kafka Streams]({{ book.kafka_streams }}/kstream/KTable)).

`buildTable` requests the given [SourceStep](SourceStep.md) for the [LogicalSchema](SourceStep.md#getSourceSchema) and [adds pseudocolumns](#withPseudoColumnsToMaterialize).

In the end, `buildTable` creates a `KTableHolder` (with the `KTable`).

---

`buildTable` is used when:

* `KSPlanBuilder` is requested to [visitTableSource](KSPlanBuilder.md#visitTableSource)
