# PlanNode

`PlanNode` is an [abstraction](#contract) of [nodes](#implementations) of a query plan.

## Contract

### <span id="buildStream"> Building SchemaKStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext)
```

Builds a [SchemaKStream](../SchemaKStream.md)

Used when:

* `PhysicalPlanBuilder` is requested to [build a PhysicalPlan](../PhysicalPlanBuilder.md#buildPhysicalPlan)
* _others_ (less important?)

### <span id="getPartitions"> Number of Partitions

```java
int getPartitions(
  KafkaTopicClient kafkaTopicClient)
```

### <span id="getSchema"> LogicalSchema

```java
LogicalSchema getSchema()
```

### <span id="getSources"> Sources

```java
List<PlanNode> getSources()
```

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
