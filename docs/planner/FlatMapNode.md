# FlatMapNode

`FlatMapNode` is a [SingleSourcePlanNode](SingleSourcePlanNode.md).

## Creating Instance

`FlatMapNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="source"> Parent [PlanNode](PlanNode.md)
* <span id="functionRegistry"> `FunctionRegistry`
* <span id="analysis"> [ImmutableAnalysis](../ImmutableAnalysis.md)

`FlatMapNode` is created when:

* `LogicalPlanner` is requested to [build a logical plan of a persistent query with a table function](LogicalPlanner.md#buildPersistentLogicalPlan-tableFunctions)

## <span id="buildStream"> Building SchemaKStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext)
```

`buildStream` requests the [source node](SingleSourcePlanNode.md#getSource) to [build a SchemaKStream](PlanNode.md#buildStream) to [flatMap](../SchemaKStream.md#flatMap).

`buildStream` is part of the [PlanNode](PlanNode.md#buildStream) abstraction.
