# KsqlBareOutputNode

`KsqlBareOutputNode` is an [OutputNode](OutputNode.md) for [queries with no sink](../parser/Query.md).

## Creating Instance

`KsqlBareOutputNode` takes the following to be created:

* <span id="id"> `PlanNodeId`
* <span id="source"> Source [PlanNode](PlanNode.md)
* <span id="schema"> `LogicalSchema`
* <span id="limit"> Limit
* <span id="timestampColumn"> `TimestampColumn`
* <span id="windowInfo"> `WindowInfo`

`KsqlBareOutputNode` is created when:

* `LogicalPlanner` is requested to [build an OutputNode](LogicalPlanner.md#buildOutputNode) (for a query with no sink)
