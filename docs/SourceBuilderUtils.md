# SourceBuilderUtils

## <span id="getValueSerde"> Building Value Serde

```java
Serde<GenericRow> getValueSerde(
  RuntimeBuildContext buildContext,
  SourceStep<?> streamSource,
  PhysicalSchema physicalSchema)
Serde<GenericRow> getValueSerde(
  RuntimeBuildContext buildContext,
  SourceStep<?> streamSource,
  PhysicalSchema physicalSchema,
  QueryContext queryContext)
```

`getValueSerde` requests the given [RuntimeBuildContext](RuntimeBuildContext.md) to [build a value serde](RuntimeBuildContext.md#buildValueSerde) (`Serde<GenericRow>`).

---

`getValueSerde` is used when:

* `SourceBuilder` is requested to [buildTableMaterialized](SourceBuilder.md#buildTableMaterialized)
* `SourceBuilderBase` is requested to [buildTable](SourceBuilderBase.md#buildTable)
* `SourceBuilderV1` is requested to [buildStream](SourceBuilderV1.md#buildStream), [buildWindowedStream](SourceBuilderV1.md#buildWindowedStream), [buildWindowedTable](SourceBuilderV1.md#buildWindowedTable)
