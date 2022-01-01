# ImmutableAnalysis

`ImmutableAnalysis` is an [abstraction](#contract) of the [query analysis results](#implementations).

This `Immutable` prefix in the name of `ImmutableAnalysis` is to denote that it is an immutable view over the AST tree of a SQL statement (as parsed using [Analyzer.Visitor](Analyzer_Visitor.md)).

## Contract (Subset)

### <span id="getTableFunctions"> getTableFunctions

```java
List<FunctionCall> getTableFunctions()
```

See [Analysis](Analysis.md#getTableFunctions)

Used when:

* `QueryAnalyzer` is requested to [analyze a Query](QueryAnalyzer.md#analyze)
* `LogicalPlanner` is requested to [buildPersistentLogicalPlan](LogicalPlanner.md#buildPersistentLogicalPlan)
* `FlatMapNode` is [created](FlatMapNode.md#tableFunctions) and [buildSchema](FlatMapNode.md#buildSchema)

### <span id="getJoin"> getJoin

```java
List<JoinInfo> getJoin()
```

See [Analysis](Analysis.md#getJoin)

Used when:

* `LogicalPlanner` is requested to [build a source node](LogicalPlanner.md#buildSourceNode)

### <span id="getSelectItems"> getSelectItems

```java
List<SelectItem> getSelectItems()
```

See [Analysis](Analysis.md#getSelectItems)

### <span id="isJoin"> isJoin

```java
boolean isJoin()
```

See [Analysis](Analysis.md#isJoin)

Used when:

* `PullQueryValidator` is [created](PullQueryValidator.md#RULES)
* `LogicalPlanner` is requested to [build a source node](LogicalPlanner.md#buildSourceNode)

## Implementations

* [Analysis](Analysis.md)
