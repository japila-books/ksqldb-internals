# LogicalPlanner

## Creating Instance

`LogicalPlanner` takes the following to be created:

* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="analysis"> `ImmutableAnalysis`
* <span id="metaStore"> `MetaStore`

`LogicalPlanner` is created when:

* `EngineExecutor` is requested to [buildAndValidateLogicalPlan](EngineExecutor.md#buildAndValidateLogicalPlan)
* `QueryEngine` is requested to [buildQueryLogicalPlan](QueryEngine.md#buildQueryLogicalPlan)

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
