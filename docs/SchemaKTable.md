# SchemaKTable

`SchemaKTable<K>` is a [SchemaKStream](SchemaKStream.md) (of `K` keys).

## Creating Instance

`SchemaKTable` takes the following to be created:

* <span id="sourceTableStep"> [ExecutionStep](ExecutionStep.md) of `KTableHolder<K>`
* <span id="schema"> [LogicalSchema](LogicalSchema.md)
* <span id="keyFormat"> [KeyFormat](KeyFormat.md)
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="functionRegistry"> [FunctionRegistry](FunctionRegistry.md)

`SchemaKTable` is created when:

* `SchemaKSourceFactory` is requested to [create a SchemaKTable](SchemaKSourceFactory.md#schemaKTable)
* `SchemaKGroupedStream` is requested to `aggregate`
* `SchemaKGroupedTable` is requested to `aggregate`
* `SchemaKTable` is requested to [select](#select), and _other operators_

## <span id="select"> select

```java
SchemaKTable<K> select(
  List<ColumnName> keyColumnNames,
  List<SelectExpression> selectExpressions,
  Stacker contextStacker,
  PlanBuildContext buildContext,
  FormatInfo valueFormat)
```

`select` is part of the [SchemaKStream](SchemaKStream.md#select) abstraction.

---

`select` creates a copy of this `SchemaKTable` with a new [sourceTableStep](#sourceTableStep) to be a [TableSelect](TableSelect.md) and [resolveSchema](#resolveSchema) with the new step.

---

`select` [creates a TableSelect step](ExecutionStepFactory.md#tableMapValues) with the [sourceTableStep](#sourceTableStep) (and the [keyFormat](#keyFormat) and the given value formats).

In the end, creates a [SchemaKTable](#creating-instance) with the `TableSelect` step (and a new [resolveSchema](#resolveSchema), etc.)
