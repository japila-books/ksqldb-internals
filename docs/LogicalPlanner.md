# LogicalPlanner

## Creating Instance

`LogicalPlanner` takes the following to be created:

* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* [ImmutableAnalysis](#analysis)
* <span id="metaStore"> `MetaStore`

`LogicalPlanner` is created when:

* `EngineExecutor` is requested to [buildAndValidateLogicalPlan](EngineExecutor.md#buildAndValidateLogicalPlan)
* `QueryEngine` is requested to [buildQueryLogicalPlan](QueryEngine.md#buildQueryLogicalPlan)

## <span id="analysis"> ImmutableAnalysis and RewrittenAnalysis

`LogicalPlanner` creates a `RewrittenAnalysis` for the given `ImmutableAnalysis` when [created](#creating-instance).

The `RewrittenAnalysis` is used when `LogicalPlanner` is requested for the following:

* [buildSourceNode](#buildSourceNode)
* [buildPersistentLogicalPlan](#buildPersistentLogicalPlan)
* [buildQueryLogicalPlan](#buildQueryLogicalPlan)
* [buildOutputNode](#buildOutputNode)
* [getWindowInfo](#getWindowInfo)
* [getSinkTopic](#getSinkTopic)
* [getSinkKeyFormat](#getSinkKeyFormat)
* [getTargetSchema](#getTargetSchema)
* [buildAggregateNode](#buildAggregateNode)
* [buildUserProjectNode](#buildUserProjectNode)
* [buildFlatMapNode](#buildFlatMapNode)
* [buildJoinKey](#buildJoinKey)
* [buildAggregateSchema](#buildAggregateSchema)

## <span id="buildPersistentLogicalPlan"> buildPersistentLogicalPlan

```java
OutputNode buildPersistentLogicalPlan()
```

`buildPersistentLogicalPlan` [buildSourceNode](#buildSourceNode).

`buildPersistentLogicalPlan`...FIXME

`buildPersistentLogicalPlan` is used when:

* `QueryEngine` is requested to [buildQueryLogicalPlan](QueryEngine.md#buildQueryLogicalPlan)

## <span id="buildQueryLogicalPlan"> Building Logical Query Plan

```java
OutputNode buildQueryLogicalPlan(
  QueryPlannerOptions queryPlannerOptions,
  boolean isScalablePush)
```

`buildQueryLogicalPlan` is used when:

* `EngineExecutor` is requested to [buildAndValidateLogicalPlan](EngineExecutor.md#buildAndValidateLogicalPlan)

### <span id="buildQueryLogicalPlan-source"> Source Node

`buildQueryLogicalPlan` [builds a source PlanNode](#buildSourceNode) (with `isWindowed` flag based on the [RewrittenAnalysis](#analysis) of the query).

### <span id="buildQueryLogicalPlan-where"> Filter Node

For a query with `WHERE` clause (per the [RewrittenAnalysis](#analysis)), `buildQueryLogicalPlan` creates a new `QueryFilterNode` to be the current [PlanNode](PlanNode.md). Otherwise, `buildQueryLogicalPlan` throws a `KsqlException` for a missing `WHERE` clause unless [getTableScansEnabled](QueryPlannerOptions.md#getTableScansEnabled) is enabled.

### <span id="buildQueryLogicalPlan-where"> Limit Node

For a non-`isScalablePush` query with `LIMIT` clause, `buildQueryLogicalPlan` [builds a limit PlanNode](#buildLimitNode) to be the current [PlanNode](PlanNode.md).

### <span id="buildQueryLogicalPlan-project"> Project Node

In the end, `buildQueryLogicalPlan` creates a `QueryProjectNode` (to be the current [PlanNode](PlanNode.md)) and [buildOutputNode](#buildOutputNode).

## <span id="buildSourceNode"> Building Source Node

```java
PlanNode buildSourceNode(
  boolean isWindowed)
```

`buildSourceNode` [buildNonJoinNode](#buildNonJoinNode) when the [RewrittenAnalysis](#analysis) is not for a join.

Otherwise, `buildSourceNode`...FIXME

`buildSourceNode` is used when:

* `LogicalPlanner` is requested to [buildPersistentLogicalPlan](#buildPersistentLogicalPlan) and [buildQueryLogicalPlan](#buildQueryLogicalPlan)

### <span id="buildNonJoinNode"> buildNonJoinNode

```java
DataSourceNode buildNonJoinNode(
  AliasedDataSource dataSource,
  boolean isWindowed,
  KsqlConfig ksqlConfig)
```

`buildNonJoinNode` creates a [DataSourceNode](DataSourceNode.md) (with a new `PlanNodeId` with `KsqlTopic` ID).

### <span id="buildJoin"> buildJoin

```java
JoinNode buildJoin(
  Join root,
  String prefix,
  boolean isWindowed)
```

`buildJoin` creates a [JoinNode](JoinNode.md).

## <span id="buildOutputNode"> Building Output Node

```java
OutputNode buildOutputNode(
  PlanNode sourcePlanNode)
```

`buildOutputNode` creates a [KsqlStructuredDataOutputNode](KsqlStructuredDataOutputNode.md) or a `KsqlBareOutputNode` based on whether this is a [QueryContainer](parser/QueryContainer.md) or not (a `Sink` to write into is defined or not), respectively.

`buildOutputNode` is used when:

* `LogicalPlanner` is requested to [buildPersistentLogicalPlan](#buildPersistentLogicalPlan) and [buildQueryLogicalPlan](#buildQueryLogicalPlan)
