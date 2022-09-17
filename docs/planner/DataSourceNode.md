# DataSourceNode

`DataSourceNode` is a [PlanNode](PlanNode.md).

## Creating Instance

`DataSourceNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="dataSource"> [DataSource](../DataSource.md)
* <span id="alias"> Source Name
* [SchemaKStreamFactory](#schemaKStreamFactory)
* <span id="isWindowed"> `isWindowed` flag
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)

`DataSourceNode` is created when:

* `LogicalPlanner` is requested to build a [join](LogicalPlanner.md#buildJoin) or a [non-join](LogicalPlanner.md#buildNonJoinNode) plan node

### <span id="schemaKStreamFactory"> SchemaKStreamFactory

`DataSourceNode` can be given a `SchemaKStreamFactory` when [created](#creating-instance). Unless given, `DataSourceNode` uses [SchemaKSourceFactory](../SchemaKSourceFactory.md#buildSource).

!!! note
    Note the difference in type names, i.e. `SchemaKStreamFactory` (with `Stream` inside) vs `SchemaKSourceFactory` (with `Source` instead).

The `SchemaKStreamFactory` is used to [create a SchemaKStream](../SchemaKSourceFactory.md#buildSource) when `DataSourceNode` is requested to [build one](#buildStream).

## <span id="buildStream"> Building SchemaKStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext)
```

`buildStream` is part of the [PlanNode](PlanNode.md#buildStream) abstraction.

---

`buildStream` requests the given `PlanBuildContext` to `buildNodeContext`.

In the end, requests the [SchemaKStreamFactory](#schemaKStreamFactory) for a [SchemaKStream](../SchemaKSourceFactory.md#buildSource).
