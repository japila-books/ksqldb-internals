# PhysicalPlanner

`PhysicalPlanner` is a new planner (alongside [ExecutionPlanner](ExecutionPlanner.md)) that [EngineExecutor](EngineExecutor.md) uses for [persistent queries](EngineExecutor.md#planQuery) when [ksql.new.query.planner.enabled](KsqlConfig.md#KSQL_NEW_QUERY_PLANNER_ENABLED) is enabled.

## <span id="buildPlan"> Building Physical Plan

```java
PhysicalPlan buildPlan(
  MetaStore metaStore,
  LogicalPlan logicalPlan)
```

`buildPlan`...FIXME

---

`buildPlan` is used when:

* `EngineExecutor` is requested to [plan a Query](EngineExecutor.md#planQuery) (with [ksql.new.query.planner.enabled](KsqlConfig.md#KSQL_NEW_QUERY_PLANNER_ENABLED) enabled)
