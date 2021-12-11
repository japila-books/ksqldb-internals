# Query

`Query` is a [Statement](Statement.md).

## Creating Instance

`Query` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="select"> `Select`
* <span id="from"> `Relation`
* <span id="window"> `WindowExpression`
* <span id="where"> `Expression`
* <span id="groupBy"> `GroupBy`
* <span id="partitionBy"> `PartitionBy`
* <span id="having"> `Expression`
* <span id="refinement"> `RefinementInfo`
* <span id="pullQuery"> `pullQuery` flag
* <span id="limit"> Limit

`Query` is created when:

* `EngineExecutor` is requested to [sourceTablePlan](EngineExecutor.md#sourceTablePlan)
* `Rewriter` is requested to `visitQuery`
* `AstBuilder.Visitor` is requested to [visitQuery](Visitor.md#visitQuery)
