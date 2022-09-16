# SingleSourcePlanNode

`SingleSourcePlanNode` is an [extension](#contract) of the [PlanNode](PlanNode.md) abstraction for [PlanNodes](#implementations) with a single [source](#source) (hence the name).

## Implementations

* `AggregateNode`
* `FilterNode`
* [FlatMapNode](FlatMapNode.md)
* [OutputNode](OutputNode.md)
* `PreJoinRepartitionNode`
* `ProjectNode`
* `QueryFilterNode`
* `QueryLimitNode`
* `SuppressNode`
* `UserRepartitionNode`

## Creating Instance

`SingleSourcePlanNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="nodeOutputType"> `DataSourceType`
* <span id="sourceName"> Source Name
* [Source](PlanNode.md)

!!! note "Abstract Class"
    `SingleSourcePlanNode` is an abstract class and cannot be created directly. It is created indirectly for the [concrete SingleSourcePlanNodes](#implementations).

## <span id="source"><span id="getSource"><span id="getSources"> Source PlanNode

`SingleSourcePlanNode` is given a single source [PlanNode](PlanNode.md) when [created](#creating-instance).

### getSources

```java
List<PlanNode> getSources()
```

`getSources` is part of the [PlanNode](PlanNode.md#getSources) abstraction.

---

`getSources` returns the [source PlanNode](#source).
