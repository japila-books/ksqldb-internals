# SchemaKStream

`SchemaKStream` (of `K` keys) is...FIXME

## Creating Instance

`SchemaKStream` takes the following to be created:

* [ExecutionStep](#sourceStep)
* <span id="schema"> [LogicalSchema](LogicalSchema.md)
* <span id="keyFormat"> [KeyFormat](KeyFormat.md)
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="functionRegistry"> [FunctionRegistry](FunctionRegistry.md)

`SchemaKStream` is created when:

* `SchemaKSourceFactory` is requested for a [SchemaKStream](SchemaKSourceFactory.md#schemaKStream)
* `SchemaKStream` is requested to [into](#into), [filter](#filter), [flatMap](#flatMap), [select](#select) and _others_

## <span id="sourceStep"><span id="getSourceStep"> Source ExecutionStep

```java
ExecutionStep<KStreamHolder<K>> sourceStep
```

`SchemaKStream` is given an [ExecutionStep](ExecutionStep.md) (of `KStreamHolder<K>`) when [created](#creating-instance).

The `ExecutionStep` is used for the following (high-level operators):

* [filter](#filter)
* [flatMap](#flatMap)
* [groupBy](#groupBy)
* [groupByKey](#groupByKey)
* [into](#into)
* [join](#join)
* [select](#select)
* [selectKey](#selectKey)

### getSourceStep

```java
ExecutionStep<?> getSourceStep()
```

`getSourceStep` returns the [source ExecutionStep](#sourceStep).

---

`getSourceStep` is used when:

* `ExecutionPlanBuilder` is requested for the [execution plan](ExecutionPlanBuilder.md#buildPhysicalPlan) (of a [Query](parser/Query.md) statement)

## <span id="select"> select

```java
SchemaKStream<K> select(
  List<ColumnName> keyColumnNames,
  List<SelectExpression> selectExpressions,
  Stacker contextStacker,
  PlanBuildContext buildContext,
  FormatInfo valueFormat)
```

`select` creates a [SchemaKStream](#creating-instance) with a [StreamSelect execution step](ExecutionStepFactory.md#streamSelect) (with the [source ExecutionStep](#sourceStep)).

---

`select` is used when:

* `AggregateNode` is requested to `selectRequiredInputColumns`, `selectRequiredOutputColumns`
* `ProjectNode` is requested to [buildStream](planner/ProjectNode.md#buildStream)
