# SchemaKStream

## Creating Instance

`SchemaKStream` takes the following to be created:

* [ExecutionStep](#sourceStep)
* <span id="schema"> `LogicalSchema`
* <span id="keyFormat"> `KeyFormat`
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="functionRegistry"> `FunctionRegistry`

`SchemaKStream` is created when:

* `SchemaKSourceFactory` is requested to [schemaKStream](SchemaKSourceFactory.md#schemaKStream)
* `SchemaKStream` is requested to [into](#into), [filter](#filter), [flatMap](#flatMap) and _others_

## <span id="sourceStep"> ExecutionStep

```java
ExecutionStep<KStreamHolder<K>> sourceStep
```

`SchemaKStream` is given an [ExecutionStep](ExecutionStep.md) (of `KStreamHolder<K>`) when [created](#creating-instance).

### <span id="getSourceStep"> getSourceStep

```java
ExecutionStep<?> getSourceStep()
```

`getSourceStep` returns the [Source ExecutionStep](#sourceStep).

---

`getSourceStep` is used when:

* `ExecutionPlanBuilder` is requested for the [execution plan](ExecutionPlanBuilder.md#buildPhysicalPlan) (of a [Query](parser/Query.md) statement)
