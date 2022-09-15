# LogicalPlanner

## Creating Instance

`LogicalPlanner` takes the following to be created:

* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* [ImmutableAnalysis](#analysis)
* <span id="metaStore"> [MetaStore](../MetaStore.md)

`LogicalPlanner` is created when:

* `EngineExecutor` is requested to [buildAndValidateLogicalPlan](../EngineExecutor.md#buildAndValidateLogicalPlan)
* `QueryEngine` is requested to [buildQueryLogicalPlan](../QueryEngine.md#buildQueryLogicalPlan)

## <span id="analysis"> ImmutableAnalysis and RewrittenAnalysis

`LogicalPlanner` creates a [RewrittenAnalysis](../analyzer/RewrittenAnalysis.md) with the following when [created](#creating-instance):

* [ImmutableAnalysis](../analyzer/ImmutableAnalysis.md)
* `ColumnReferenceRewriter.process` as the rewriter

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

In summary, `buildPersistentLogicalPlan` creates an [OutputNode](OutputNode.md) for the [RewrittenAnalysis](#analysis) (of a [Query](../parser/Query.md) statement).

---

`buildPersistentLogicalPlan` is used when:

* `QueryEngine` is requested to [build a logical plan of a query](../QueryEngine.md#buildQueryLogicalPlan)

### <span id="buildPersistentLogicalPlan-source"> Step 1. Source Node

`buildPersistentLogicalPlan` [builds a source node](#buildSourceNode) (with `isWindowed` flag based on the [RewrittenAnalysis](#analysis) of the query).

### <span id="buildPersistentLogicalPlan-where"> Step 2. FilterNode

For a query with `WHERE` clause (per the [RewrittenAnalysis](#analysis)), `buildPersistentLogicalPlan` creates a new `QueryFilterNode` to be the current [PlanNode](PlanNode.md).

### <span id="buildPersistentLogicalPlan-partitionBy"> Step 3. UserRepartitionNode

For a query with `PartitionBy` clause (per the [RewrittenAnalysis](#analysis)), `buildPersistentLogicalPlan` [buildUserRepartitionNode](#buildUserRepartitionNode).

### <span id="buildPersistentLogicalPlan-tableFunctions"> Step 4. FlatMapNode

For a query with `TableFunctions` (per the [RewrittenAnalysis](#analysis)), `buildPersistentLogicalPlan` [buildFlatMapNode](#buildFlatMapNode).

#### <span id="buildFlatMapNode"> Building FlatMapNode

```java
FlatMapNode buildFlatMapNode(
  PlanNode sourcePlanNode)
```

`buildFlatMapNode` creates a [FlatMapNode](FlatMapNode.md) (with a `PlanNodeId` with `FlatMap` ID).

### <span id="buildPersistentLogicalPlan-groupBy"> AggregateNode

For a query with `GroupBy` clause (per the [RewrittenAnalysis](#analysis)), `buildPersistentLogicalPlan` [buildAggregateNode](#buildAggregateNode). Otherwise, `buildPersistentLogicalPlan`...FIXME

### <span id="buildPersistentLogicalPlan-RefinementInfo"> RefinementInfo

For a query with a `RefinementInfo` (per the [RewrittenAnalysis](#analysis)), `buildPersistentLogicalPlan`...FIXME

### <span id="buildPersistentLogicalPlan-OutputNode"> OutputNode

In the end, `buildPersistentLogicalPlan` [builds an output node](#buildOutputNode).

## <span id="buildQueryLogicalPlan"> Building Logical Plan of Query (buildQueryLogicalPlan)

```java
OutputNode buildQueryLogicalPlan(
  QueryPlannerOptions queryPlannerOptions,
  boolean isScalablePush)
```

`buildQueryLogicalPlan` is used when:

* `EngineExecutor` is requested to [buildAndValidateLogicalPlan](../EngineExecutor.md#buildAndValidateLogicalPlan)

### <span id="buildQueryLogicalPlan-source"> Step 1. Source Node

`buildQueryLogicalPlan` [builds a source node](#buildSourceNode) (with `isWindowed` flag based on the [RewrittenAnalysis](#analysis) of the query).

### <span id="buildQueryLogicalPlan-where"> Step 2. Filter Node

For a query with `WHERE` clause (per the [RewrittenAnalysis](#analysis)), `buildQueryLogicalPlan` creates a new `QueryFilterNode` to be the current [PlanNode](PlanNode.md). Otherwise, `buildQueryLogicalPlan` throws a `KsqlException` for a missing `WHERE` clause unless [getTableScansEnabled](QueryPlannerOptions.md#getTableScansEnabled) is enabled.

### <span id="buildQueryLogicalPlan-limit"> Step 3. Limit Node

For a non-`isScalablePush` query with `LIMIT` clause, `buildQueryLogicalPlan` [builds a limit PlanNode](#buildLimitNode) to be the current [PlanNode](PlanNode.md).

### <span id="buildQueryLogicalPlan-project"> Step 4. Project Node

In the end, `buildQueryLogicalPlan` [builds an output node](#buildOutputNode) with a [QueryProjectNode](QueryProjectNode.md).

## <span id="buildSourceNode"> Building DataSourceNode or JoinNode (buildSourceNode)

```java
PlanNode buildSourceNode(
  boolean isWindowed)
```

`buildSourceNode` [buildNonJoinNode](#buildNonJoinNode) when the [RewrittenAnalysis](#analysis) is not a join.

Otherwise, `buildSourceNode`...FIXME

---

`buildSourceNode` is used when:

* `LogicalPlanner` is requested to [buildPersistentLogicalPlan](#buildPersistentLogicalPlan) and [buildQueryLogicalPlan](#buildQueryLogicalPlan)

### <span id="buildNonJoinNode"> Creating DataSourceNode (buildNonJoinNode)

```java
DataSourceNode buildNonJoinNode(
  AliasedDataSource dataSource,
  boolean isWindowed,
  KsqlConfig ksqlConfig)
```

`buildNonJoinNode` creates a [DataSourceNode](DataSourceNode.md) (with a new `PlanNodeId` with `KsqlTopic` ID).

### <span id="buildJoin"> Creating JoinNode (buildJoin)

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

`buildOutputNode` creates one of the two possible [OutputNode](OutputNode.md)s:

* [KsqlBareOutputNode](KsqlBareOutputNode.md) when the [RewrittenAnalysis](#analysis) has no [Into](../analyzer/RewrittenAnalysis.md#getInto) sink
* [KsqlStructuredDataOutputNode](KsqlStructuredDataOutputNode.md), otherwise

---

`buildOutputNode` is used when:

* `LogicalPlanner` is requested to [buildPersistentLogicalPlan](#buildPersistentLogicalPlan) and [buildQueryLogicalPlan](#buildQueryLogicalPlan)
