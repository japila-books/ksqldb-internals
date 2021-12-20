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
* [DataSourceType](#nodeOutputType)
* <span id="sourceName"> Source Name

!!! note "Abstract Class"
    `PlanNode` is an abstract class and cannot be created directly. It is created indirectly for the [concrete PlanNodes](#implementations).

### <span id="DataSourceType"><span id="nodeOutputType"> DataSourceType

`PlanNode` is given a `DataSourceType` when [created](#creating-instance).

DataSourceType | ksqlType
---------------|---------
 KSTREAM | STREAM
 KTABLE | TABLE
