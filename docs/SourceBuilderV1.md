# SourceBuilderV1

`SourceBuilderV1` is a [SourceBuilderBase](SourceBuilderBase.md).

## Creating Instance

`SourceBuilderV1` takes no arguments to be created.

`SourceBuilderV1` is created when the Java class is first loaded by JVM (and available using [instance](#instance) public static value).

## <span id="instance"> SourceBuilderV1 Instance

`SourceBuilderV1` uses an `instance` internal registry of the only application-wide `SourceBuilderV1` (that is [created](#creating-instance) right when the class is loaded by JVM).

!!! note
    `instance` is a `private static final` value.

`instance` is used when:

* `KSPlanBuilder` is requested to [visitStreamSource](KSPlanBuilder.md#visitStreamSource), [visitWindowedStreamSource](KSPlanBuilder.md#visitWindowedStreamSource), [visitTableSource](KSPlanBuilder.md#visitTableSource) and [visitWindowedTableSource](KSPlanBuilder.md#visitWindowedTableSource)

## <span id="buildStream"> buildStream

```java
KStreamHolder<GenericKey> buildStream(
  RuntimeBuildContext buildContext,
  StreamSource source,
  ConsumedFactory consumedFactory)
```

`buildStream` [gets a PhysicalSchema](#getPhysicalSchema) for the given [StreamSource](StreamSource.md).

`buildStream` [getValueSerde](#getValueSerde), [getKeySerde](#getKeySerde) and [builds a Consumed](#buildSourceConsumed).

`buildStream` [builds a KStream](#buildKStream) (with the given [StreamSource](StreamSource.md), the `Consumed` and a non-windowed `KeyGenerator`).

In the end, `buildStream` creates a `KStreamHolder` for the `KStream`.

`buildStream` is used when:

* `KSPlanBuilder` is requested to [visitStreamSource](KSPlanBuilder.md#visitStreamSource)

## <span id="buildKStream"> buildKStream

```java
KStream<K, GenericRow> buildKStream(
  SourceStep<?> streamSource,
  RuntimeBuildContext buildContext,
  Consumed<K, GenericRow> consumed,
  Function<K, Collection<?>> keyGenerator)
```

`buildKStream`...FIXME

`buildKStream` is used when:

* `SourceBuilderV1` is requested to [buildStream](#buildStream) and [buildWindowedStream](#buildWindowedStream)
