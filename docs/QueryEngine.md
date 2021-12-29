# QueryEngine

## Creating Instance

`QueryEngine` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="processingLogContext"> `ProcessingLogContext`

`QueryEngine` is created when:

* `EngineContext` is requested to [create one](EngineContext.md#createQueryEngine)

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

The optional `Sink` can only be defined when `EngineExecutor` is requested to [plan a statement](EngineExecutor.md#plan) (which is a [QueryContainer](parser/QueryContainer.md#getSink)).

---

`buildQueryLogicalPlan` is used when:

* `EngineExecutor` is requested to [planQuery](EngineExecutor.md#planQuery)

## <span id="buildPhysicalPlan"> Building Physical Query Plan (buildPhysicalPlan)

```java
PhysicalPlan buildPhysicalPlan(
  LogicalPlanNode logicalPlanNode,
  SessionConfig config,
  MetaStore metaStore,
  QueryId queryId,
  Optional<PlanInfo> oldPlanInfo)
```

`buildPhysicalPlan` creates a `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder)).

`buildPhysicalPlan` creates a [PhysicalPlanBuilder](PhysicalPlanBuilder.md) (with the `StreamsBuilder`) to [build a physical plan](PhysicalPlanBuilder.md#buildPhysicalPlan) of the given `LogicalPlanNode` (and the given `QueryId` and the `PlanInfo` of the query to be "replaced").

`buildPhysicalPlan` is used when:

* `EngineExecutor` is requested to [plan a query](EngineExecutor.md#planQuery)
