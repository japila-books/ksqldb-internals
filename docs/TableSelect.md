# TableSelect

`TableSelect` is a [ExecutionStep](ExecutionStep.md) of `KTableHolder<K>`.

## Creating Instance

`TableSelect` takes the following to be created:

* <span id="props"> `ExecutionStepPropertiesV1`
* <span id="source"> Source [ExecutionStep](ExecutionStep.md) of `KTableHolder<K>`
* <span id="keyColumnNames"> Key column names
* <span id="selectExpressions"> Select Expressions
* <span id="internalFormats"> Internal `Formats`

`TableSelect` is created when:

* `ExecutionStepFactory` is requested to [create a TableSelect](ExecutionStepFactory.md#tableMapValues)

## <span id="build"> Building KTableHolder

```java
KTableHolder<K> build(
  PlanBuilder builder,
  PlanInfo planInfo)
```

`build` is part of the [ExecutionStep](ExecutionStep.md#build) abstraction.

---

`build` requests the given [PlanBuilder](PlanBuilder.md) to [visit this TableSelect](PlanBuilder.md#visitTableSelect).
