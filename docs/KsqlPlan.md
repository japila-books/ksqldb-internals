# KsqlPlan

## <span id="ddlPlanCurrent"> ddlPlanCurrent

```java
KsqlPlan ddlPlanCurrent(
  String statementText,
  DdlCommand ddlCommand)
```

`ddlPlanCurrent` creates a [KsqlPlanV1](KsqlPlanV1.md) with the given `statementText` and the `DdlCommand`.

`ddlPlanCurrent` is used when:

* `EngineExecutor` is requested to [plan a DdlStatement](EngineExecutor.md#plan) (for a non-source table)
