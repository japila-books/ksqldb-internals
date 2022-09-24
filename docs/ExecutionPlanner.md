# ExecutionPlanner

`ExecutionPlanner` is a new planner (alongside [PhysicalPlanner](PhysicalPlanner.md)) that [EngineExecutor](EngineExecutor.md) uses for [persistent queries](EngineExecutor.md#planQuery) when [ksql.new.query.planner.enabled](KsqlConfig.md#KSQL_NEW_QUERY_PLANNER_ENABLED) is enabled.

## <span id="buildPlan"> Building Execution Plan

```java
ExecutionPlan buildPlan(
  MetaStore metaStore,
  PhysicalPlan physicalPlan,
  Sink sink)
```

`buildPlan`...FIXME

---

`buildPlan` is used when:

* `EngineExecutor` is requested to [plan a Query](EngineExecutor.md#planQuery) (with [ksql.new.query.planner.enabled](KsqlConfig.md#KSQL_NEW_QUERY_PLANNER_ENABLED) enabled)
