# ProcessingLogContextImpl

`ProcessingLogContextImpl` is a [ProcessingLogContext](ProcessingLogContext.md).

## Creating Instance

`ProcessingLogContextImpl` takes the following to be created:

* <span id="config"> [ProcessingLogConfig](ProcessingLogConfig.md)
* <span id="metrics"> `Metrics`
* <span id="metricsTags"> Metrics Tags

`ProcessingLogContextImpl` is created when:

* `ProcessingLogContext` is requested to [create a ProcessingLogContext](ProcessingLogContext.md#create)

### <span id="loggerFactory"><span id="getLoggerFactory"> MeteredProcessingLoggerFactory

`ProcessingLogContextImpl` creates a [MeteredProcessingLoggerFactory](MeteredProcessingLoggerFactory.md) when [created](#creating-instance).

Used when:

* `PullPhysicalPlanBuilder` is requested to [translateProjectNode](../PullPhysicalPlanBuilder.md#translateProjectNode), [translateFilterNode](../PullPhysicalPlanBuilder.md#translateFilterNode)
* `PushPhysicalPlanBuilder` is requested to [translateProjectNode](../PushPhysicalPlanBuilder.md#translateProjectNode), [translateFilterNode](../PushPhysicalPlanBuilder.md#translateFilterNode)
* `QueryBuilder` is requested to [buildTransientQuery](../QueryBuilder.md#buildTransientQuery), [buildPersistentQueryInDedicatedRuntime](../QueryBuilder.md#buildPersistentQueryInDedicatedRuntime), [buildPersistentQueryInSharedRuntime](../QueryBuilder.md#buildPersistentQueryInSharedRuntime), [buildStreamsProperties](../QueryBuilder.md#buildStreamsProperties)
