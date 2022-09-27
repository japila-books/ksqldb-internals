# TableSource

`TableSource` is a [SourceStep](SourceStep.md) of `KTableHolder` of `GenericKey`s.

## Creating Instance

`TableSource` takes the following to be created:

* <span id="props"> `ExecutionStepPropertiesV1`
* <span id="topicName"> Topic Name
* <span id="formats"> `Formats`
* <span id="timestampColumn"> `TimestampColumn`
* <span id="sourceSchema"> [LogicalSchema](LogicalSchema.md)
* <span id="pseudoColumnVersion"> Pseudo Column Version
* <span id="stateStoreFormats"> StateStore Formats

`TableSource` is created when:

* `ExecutionStepFactory` is requested for a [TableSource](ExecutionStepFactory.md#tableSource)

## <span id="build"> Building KTableHolder

```java
KTableHolder<GenericKey> build(
  PlanBuilder builder,
  PlanInfo info)
```

`build` is part of the [ExecutionStep](ExecutionStep.md#build) abstraction.

---

`build` requests the given [PlanBuilder](PlanBuilder.md) to [visit this TableSource](PlanBuilder.md#visitTableSource).
