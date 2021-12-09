# DataSourceNode

## Creating Instance

`DataSourceNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="dataSource"> `DataSource`
* <span id="alias"> `SourceName`
* [SchemaKStreamFactory](#schemaKStreamFactory)
* <span id="isWindowed"> `isWindowed` flag
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)

`DataSourceNode` is created when:

* `LogicalPlanner` is requested to [buildJoin](LogicalPlanner.md#buildJoin) and [buildNonJoinNode](LogicalPlanner.md#buildNonJoinNode)

## <span id="schemaKStreamFactory"> SchemaKStreamFactory

`DataSourceNode` can be given a `SchemaKStreamFactory` when [created](#creating-instance). Unless given, `DataSourceNode` uses [SchemaKSourceFactory](SchemaKSourceFactory.md#buildSource).
