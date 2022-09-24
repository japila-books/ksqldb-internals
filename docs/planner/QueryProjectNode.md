# QueryProjectNode

`QueryProjectNode` is a [ProjectNode](ProjectNode.md) that is the [top-level PlanNode](LogicalPlanner.md#buildQueryLogicalPlan-project) in a query logical plan (after `LogicalPlanner` is requested to [build a query logical plan](LogicalPlanner.md#buildQueryLogicalPlan)).

## Creating Instance

`QueryProjectNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="source"> Source [PlanNode](PlanNode.md)
* <span id="selectItems"> `SelectItem`s
* <span id="metaStore"> [MetaStore](../MetaStore.md)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="analysis"> [RewrittenAnalysis](../analyzer/RewrittenAnalysis.md)
* <span id="isWindowed"> `isWindowed` flag
* <span id="queryPlannerOptions"> [QueryPlannerOptions](QueryPlannerOptions.md)
* <span id="isScalablePush"> `isScalablePush` flag

`QueryProjectNode` is created when:

* `LogicalPlanner` is requested to [buildQueryLogicalPlan](LogicalPlanner.md#buildQueryLogicalPlan)
