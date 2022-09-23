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

* `ExecutionPlanBuilder` is requested for a [PhysicalPlan](../ExecutionPlanBuilder.md#buildPhysicalPlan)
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

### <span id="getSources"> Source PlanNodes

```java
List<PlanNode> getSources()
```

Source [PlanNode](PlanNode.md)s:

* No sources for [DataSourceNode](DataSourceNode.md#getSources)
* The single source of [SingleSourcePlanNode](SingleSourcePlanNode.md#getSources)
* The left and right join sources of [JoinNode](JoinNode.md#getSources)

Used when:

* `PullPhysicalPlanBuilder` is requested to `buildPullPhysicalPlan`
* `PushPhysicalPlanBuilder` is requested to `buildPushPhysicalPlan`
* `JoinNode` is requested to [findUpstreamJoin](JoinNode.md#findUpstreamJoin), [getPreJoinProjectDataSources](JoinNode.md#getPreJoinProjectDataSources)
* `PlanNode` is requested to [getSourceNodes](#getSourceNodes), [resolveSelectStar](#resolveSelectStar), [validateColumns](#validateColumns), [validateKeyPresent](#validateKeyPresent)

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

## <span id="getSourceNodes"> getSourceNodes

```java
Stream<DataSourceNode> getSourceNodes()
```

For this `PlanNode` as [DataSourceNode](DataSourceNode.md), `getSourceNodes` simply returns this `DataSourceNode`.

Otherwise, `getSourceNodes` takes the [source PlanNodes](#getSources) and then their `DataSourceNode`s.

---

`getSourceNodes` is used when:

* `EngineExecutor` is requested to [getSourceNames](../EngineExecutor.md#getSourceNames)
* `LogicalPlanner` is requested to [joinOnNonKeyAttribute](LogicalPlanner.md#joinOnNonKeyAttribute)
* `JoinNode` is requested to [getDefaultSourceKeyFormat](JoinNode.md#getDefaultSourceKeyFormat)
* `PlanNode` is requested to [getLeftmostSourceNode](#getLeftmostSourceNode)
* _a few others_
