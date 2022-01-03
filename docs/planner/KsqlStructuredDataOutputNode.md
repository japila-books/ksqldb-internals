# KsqlStructuredDataOutputNode

`KsqlStructuredDataOutputNode` is an [OutputNode](OutputNode.md).

## Creating Instance

`KsqlStructuredDataOutputNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="source"> Parent [PlanNode](PlanNode.md)
* <span id="schema"> `LogicalSchema`
* <span id="timestampColumn"> `TimestampColumn`
* <span id="ksqlTopic"> `KsqlTopic`
* <span id="limit"> Limit
* <span id="doCreateInto"><span id="createInto"> `doCreateInto` flag
* <span id="sinkName"> Sink Name
* <span id="orReplace"> `orReplace` flag

`KsqlStructuredDataOutputNode` is created when:

* `LogicalPlanner` is requested to [build an OutputNode](LogicalPlanner.md#buildOutputNode) (for a [QueryContainer](../parser/QueryContainer.md))

## <span id="buildStream"> Building SchemaKStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext)
```

`buildStream` requests the [source PlanNode](SingleSourcePlanNode.md#getSource) to [build a SchemaKStream](#buildStream).

In the end, `buildStream` requests the [SchemaKStream](../SchemaKStream.md) to [into](../SchemaKStream.md#into) (for the [ksqlTopic](#ksqlTopic) and the [timestampColumn](OutputNode.md#timestampColumn)).

---

`buildStream` is part of the [PlanNode](PlanNode.md#buildStream) abstraction.
