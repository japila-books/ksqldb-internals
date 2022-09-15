# QueryProjectNode

`QueryProjectNode` is a `ProjectNode`.

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
