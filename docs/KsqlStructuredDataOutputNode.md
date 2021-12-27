# KsqlStructuredDataOutputNode

`KsqlStructuredDataOutputNode` is an [OutputNode](OutputNode.md).

## Creating Instance

`KsqlStructuredDataOutputNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="source"> [PlanNode](PlanNode.md)
* <span id="schema"> `LogicalSchema`
* <span id="timestampColumn"> `TimestampColumn`
* <span id="ksqlTopic"> `KsqlTopic`
* <span id="limit"> Limit
* <span id="doCreateInto"> `doCreateInto` flag
* <span id="sinkName"> Sink Name
* <span id="orReplace"> `orReplace` flag

`KsqlStructuredDataOutputNode` is created when:

* `LogicalPlanner` is requested to [build an OutputNode](LogicalPlanner.md#buildOutputNode)
