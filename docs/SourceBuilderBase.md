# SourceBuilderBase

`SourceBuilderBase` is an [abstraction](#contract) of [source builders](#implementations).

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

## Implementations

* [SourceBuilder](SourceBuilder.md)
* [SourceBuilderV1](SourceBuilderV1.md)
