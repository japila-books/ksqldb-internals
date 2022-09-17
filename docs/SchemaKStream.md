# SchemaKStream

## Creating Instance

`SchemaKStream` takes the following to be created:

* [ExecutionStep](#sourceStep)
* <span id="schema"> `LogicalSchema`
* <span id="keyFormat"> `KeyFormat`
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="functionRegistry"> [FunctionRegistry](FunctionRegistry.md)

`SchemaKStream` is created when:

* `SchemaKSourceFactory` is requested for a [SchemaKStream](SchemaKSourceFactory.md#schemaKStream)
* `SchemaKStream` is requested to [into](#into), [filter](#filter), [flatMap](#flatMap) and _others_

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
