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
* [pullQuery](#pullQuery)
* <span id="limit"> Limit

`Query` is created when:

* `EngineExecutor` is requested to [sourceTablePlan](../EngineExecutor.md#sourceTablePlan)
* `StatementRewriter.Rewriter` is requested to `visitQuery`
* `AstBuilder.Visitor` is requested to [visitQuery](AstBuilder_Visitor.md#visitQuery)

## <span id="pullQuery"> pullQuery Flag

`Query` is given a `pullQuery` flag when [created](#creating-instance) (which is most importantly when `AstBuilder.Visitor` is requested to [visitQuery](AstBuilder_Visitor.md#visitQuery)).

`AstBuilder.Visitor` turns the `pullQuery` flag on (`true`) when the `Query` has no `EMIT` clause (and the [buildingPersistentQuery](AstBuilder_Visitor.md#buildingPersistentQuery) internal flag is off).

### <span id="isPullQuery"> isPullQuery

```java
boolean isPullQuery()
```

`isPullQuery` is used when:

* `Analyzer.Visitor` is requested to [visitQuery](../Analyzer_Visitor.md#visitQuery)
* `QueryAnalyzer` is requested to [analyze](../QueryAnalyzer.md#analyze)
* `EngineExecutor` is requested to [executeTablePullQuery](../EngineExecutor.md#executeTablePullQuery)
* `StatementRewriter.Rewriter` is requested to `visitQuery`
* `SqlFormatter.Formatter` is requested to `visitQuery`
* `QueryExecutor` is requested to [handleQuery](../rest/QueryExecutor.md#handleQuery)
* `ScalablePushUtil` is requested to [isScalablePushQuery](../rest/ScalablePushUtil.md#isScalablePushQuery)

## <span id="accept"> accept

```java
R accept(
  AstVisitor<R, C> visitor,
  C context)
```

`accept` requests the given [AstVisitor](AstVisitor.md) to [visit a Query](AstVisitor.md#visitQuery).

`accept` is part of the [AstNode](AstNode.md#accept) abstraction.
