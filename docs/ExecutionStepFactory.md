# ExecutionStepFactory

## <span id="streamSource"> Creating StreamSource

```java
StreamSource streamSource(
  QueryContext.Stacker stacker,
  LogicalSchema sourceSchema,
  String topicName,
  Formats formats,
  Optional<TimestampColumn> timestampColumn,
  int pseudoColumnVersion)
```

`streamSource` creates a [StreamSource](StreamSource.md) (with `ExecutionStepPropertiesV1`).

---

`streamSource` is used when:

* `SchemaKSourceFactory` is requested to [buildStream](SchemaKSourceFactory.md#buildStream)

## <span id="tableMapValues"> Creating TableSelect

```java
<K> TableSelect<K> tableMapValues(
  QueryContext.Stacker stacker,
  ExecutionStep<KTableHolder<K>> source,
  List<ColumnName> keyColumnNames,
  List<SelectExpression> selectExpressions,
  Formats format)
```

`tableMapValues` creates a [TableSelect](TableSelect.md) (with a new `ExecutionStepPropertiesV1`).

---

`tableMapValues` is used when:

* `SchemaKTable` is requested to [select](SchemaKTable.md#select)
