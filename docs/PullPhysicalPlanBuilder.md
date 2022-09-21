# PullPhysicalPlanBuilder

## Creating Instance

`PullPhysicalPlanBuilder` takes the following to be created:

* <span id="processingLogContext"> [ProcessingLogContext](monitoring/ProcessingLogContext.md)
* <span id="persistentQueryMetadata"> [PersistentQueryMetadata](PersistentQueryMetadata.md)
* <span id="analysis"> [ImmutableAnalysis](analyzer/ImmutableAnalysis.md)
* <span id="queryPlannerOptions"> [QueryPlannerOptions](planner/QueryPlannerOptions.md)
* <span id="shouldCancelOperations"> Should Cancel Ops
* <span id="consistencyOffsetVector"> `ConsistencyOffsetVector`

`PullPhysicalPlanBuilder` is created when:

* `EngineExecutor` is requested to [build a physical plan for a pull query](EngineExecutor.md#buildPullPhysicalPlan)
