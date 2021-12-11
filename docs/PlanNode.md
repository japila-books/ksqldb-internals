# PlanNode

`PlanNode` is an [abstraction](#contract) of [nodes](#implementations) of a query plan.

## Contract (Subset)

### <span id="buildStream"> buildStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext)
```

Builds a [SchemaKStream](SchemaKStream.md)

## Implementations

* [DataSourceNode](DataSourceNode.md)
* [JoinNode](JoinNode.md)
* [SingleSourcePlanNode](SingleSourcePlanNode.md)

## Creating Instance

`PlanNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="nodeOutputType"> `DataSourceType`
* <span id="sourceName"> Source Name

!!! note "Abstract Class"
    `PlanNode` is an abstract class and cannot be created directly. It is created indirectly for the [concrete PlanNodes](#implementations).
