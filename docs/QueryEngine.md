# QueryEngine

## <span id="buildQueryLogicalPlan"> Building Logical Query Plan (buildQueryLogicalPlan)

```java
OutputNode buildQueryLogicalPlan(
  Query query,
  Optional<Sink> sink,
  MetaStore metaStore,
  KsqlConfig config,
  boolean rowpartitionRowoffsetEnabled)
```

`buildQueryLogicalPlan` creates a [QueryAnalyzer](QueryAnalyzer.md) with the [MetaStore](MetaStore.md) and the values of the following configuration properties (from the given [KsqlConfig](KsqlConfig.md)):

* [KSQL_OUTPUT_TOPIC_NAME_PREFIX_CONFIG](KsqlConfig.md#KSQL_OUTPUT_TOPIC_NAME_PREFIX_CONFIG)
* [KSQL_QUERY_PULL_LIMIT_CLAUSE_ENABLED](KsqlConfig.md#KSQL_QUERY_PULL_LIMIT_CLAUSE_ENABLED)

`buildQueryLogicalPlan` requests the `QueryAnalyzer` to [analyze the given query](QueryAnalyzer.md#analyze).

In the end, `buildQueryLogicalPlan` creates a [LogicalPlanner](LogicalPlanner.md) to [buildPersistentLogicalPlan](LogicalPlanner.md#buildPersistentLogicalPlan).

---

The optional `Sink` can only be defined when `EngineExecutor` is requested to [plan a statement](EngineExecutor.md#plan) (which is a [QueryContainer](QueryContainer.md#getSink)).

---

`buildQueryLogicalPlan` is used when:

* `EngineExecutor` is requested to [planQuery](EngineExecutor.md#planQuery)
