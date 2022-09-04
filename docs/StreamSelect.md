# StreamSelect

`StreamSelect<K>` is an [ExecutionStep](ExecutionStep.md) (of `KStreamHolder`).

## Creating Instance

`StreamSelect` takes the following to be created:

* <span id="props"> `ExecutionStepPropertiesV1`
* <span id="source"> Source [ExecutionStep](ExecutionStep.md)
* <span id="keyColumnNames"> Key column names (`List<ColumnName>`)
* <span id="selectedKeys"> Select keys (`Optional<List<ColumnName>>`)
* <span id="selectExpressions"> Select Expressions (`List<SelectExpression>`)

`StreamSelect` is created when:

* `ExecutionStepFactory` is requested to [streamSelect](ExecutionStepFactory.md#streamSelect)

## <span id="build"> Building KStreamHolder

```java
KStreamHolder<K> build(
  PlanBuilder builder,
  PlanInfo info)
```

`build` is part of the [ExecutionStep](ExecutionStep.md#build) abstraction.

---

`build` requests the given [PlanBuilder](PlanBuilder.md) to [visit a StreamSelect](PlanBuilder.md#visitStreamSelect).
