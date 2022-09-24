# QueryEngine

## Creating Instance

`QueryEngine` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="processingLogContext"> `ProcessingLogContext`

`QueryEngine` is created when:

* `EngineContext` is requested to [create one](EngineContext.md#createQueryEngine) (when `EngineExecutor` is requested to [plan a Query for execution](#planQuery))

## <span id="buildQueryLogicalPlan"> Building Logical Plan of Query (buildQueryLogicalPlan)

```java
OutputNode buildQueryLogicalPlan(
  Query query,
  Optional<Sink> sink,
  MetaStore metaStore,
  KsqlConfig config,
  boolean rowpartitionRowoffsetEnabled)
```

In summary, `buildQueryLogicalPlan` takes a [Query](parser/Query.md) statement and returns an [OutputNode](planner/OutputNode.md).

---

The optional `Sink` is only defined when `EngineExecutor` is requested to [plan a statement](EngineExecutor.md#plan) (which is a [QueryContainer](parser/QueryContainer.md#getSink)).

---

`buildQueryLogicalPlan` creates a [QueryAnalyzer](analyzer/QueryAnalyzer.md) with the [MetaStore](MetaStore.md) and the values of the following configuration properties (from the given [KsqlConfig](KsqlConfig.md)):

* [ksql.output.topic.name.prefix](KsqlConfig.md#KSQL_OUTPUT_TOPIC_NAME_PREFIX_CONFIG)
* [ksql.query.pull.limit.clause.enabled](KsqlConfig.md#KSQL_QUERY_PULL_LIMIT_CLAUSE_ENABLED)

`buildQueryLogicalPlan` requests the `QueryAnalyzer` to [analyze the given query](analyzer/QueryAnalyzer.md#analyze).

In the end, `buildQueryLogicalPlan` creates a [LogicalPlanner](planner/LogicalPlanner.md) to [buildPersistentLogicalPlan](planner/LogicalPlanner.md#buildPersistentLogicalPlan).

---

`buildQueryLogicalPlan` is used when:

* `EngineExecutor` is requested to [plan a query](EngineExecutor.md#planQuery)

## <span id="buildPhysicalPlan"> Building Physical Query Plan

```java
PhysicalPlan buildPhysicalPlan(
  LogicalPlanNode logicalPlanNode,
  SessionConfig config,
  MetaStore metaStore,
  QueryId queryId,
  Optional<PlanInfo> oldPlanInfo)
```

`buildPhysicalPlan` creates a `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder)).

`buildPhysicalPlan` creates a [ExecutionPlanBuilder](ExecutionPlanBuilder.md) (with the `StreamsBuilder`).

In the end, `buildPhysicalPlan` requests the `ExecutionPlanBuilder` to [build a ExecutionPlan](ExecutionPlanBuilder.md#buildPhysicalPlan) for the given `LogicalPlanNode` (and the given `QueryId` and the current `PlanInfo` of the query to be "replaced").

---

`buildPhysicalPlan` is used when:

* `EngineExecutor` is requested to [plan a query](EngineExecutor.md#planQuery)
