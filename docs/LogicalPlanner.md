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

`buildPersistentLogicalPlan`...FIXME

`buildPersistentLogicalPlan` is used when:

* `QueryEngine` is requested to [buildQueryLogicalPlan](QueryEngine.md#buildQueryLogicalPlan)

## <span id="buildQueryLogicalPlan"> buildQueryLogicalPlan

```java
OutputNode buildQueryLogicalPlan(
  QueryPlannerOptions queryPlannerOptions,
  boolean isScalablePush)
```

`buildQueryLogicalPlan`...FIXME

`buildQueryLogicalPlan` is used when:

* `EngineExecutor` is requested to [buildAndValidateLogicalPlan](EngineExecutor.md#buildAndValidateLogicalPlan)

## <span id="buildSourceNode"> buildSourceNode

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
