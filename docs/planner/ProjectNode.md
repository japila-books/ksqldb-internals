# ProjectNode

`ProjectNode` is an [extension](#contract) of the [SingleSourcePlanNode](SingleSourcePlanNode.md) abstraction for [project nodes](#implementations) with [select expressions](#getSelectExpressions) (for [SchemaKStream.select](../SchemaKStream.md#select) when [building a SchemaKStream](#buildStream)).

## Contract

### <span id="getSelectExpressions"> Select Expressions

```java
List<SelectExpression> getSelectExpressions()
```

Used when:

* `ProjectNode` is requested to [buildStream](#buildStream)
* `ProjectOperator` is requested to `open` and `createRowForSelectStar`

## Implementations

* `FinalProjectNode`
* `PreJoinProjectNode`
* [QueryProjectNode](QueryProjectNode.md)

## Creating Instance

`ProjectNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="source"><span id="getSource"> Source [PlanNode](PlanNode.md)

!!! note "Abstract Class"
    `ProjectNode` is an abstract class and cannot be created directly. It is created indirectly for the [concrete ProjectNodes](#implementations).

## <span id="buildStream"> buildStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext)
```

`buildStream` is part of the [PlanNode](PlanNode.md#buildStream) abstraction.

---

`buildStream` requests the [source PlanNode](#source) to [build a SchemaKStream](PlanNode.md#buildStream).

In the end, `buildStream` requests the [SchemaKStream](../SchemaKStream.md) to [select](#select).
