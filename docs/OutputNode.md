# OutputNode

`OutputNode` is an [extension](#contract) of the [SingleSourcePlanNode](SingleSourcePlanNode.md) abstraction for [output nodes](#implementations).

## Contract

### <span id="getSinkName"> Optional Sink Name

```java
Optional<SourceName> getSinkName()
```

The name of a stream or table (to write into)

Used when:

* `CreateSourceFactory` is requested to [createStreamCommand](CreateSourceFactory.md#createStreamCommand) and [createTableCommand](CreateSourceFactory.md#createTableCommand)
* `EngineExecutor` is requested to [plan a statement](EngineExecutor.md#plan), [maybeCreateSinkDdl](EngineExecutor.md#maybeCreateSinkDdl), [validateExistingSink](EngineExecutor.md#validateExistingSink)
* `QueryIdUtil` is requested to `buildId`

## Implementations

* `KsqlBareOutputNode`
* [KsqlStructuredDataOutputNode](KsqlStructuredDataOutputNode.md)

## Creating Instance

`OutputNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="source"> Source [PlanNode](PlanNode.md)
* <span id="schema"> `LogicalSchema`
* <span id="limit"> Limit
* <span id="timestampColumn"> `TimestampColumn`

!!! note "Abstract Class"
    `OutputNode` is an abstract class and cannot be created directly. It is created indirectly for the [concrete OutputNodes](#implementations).
