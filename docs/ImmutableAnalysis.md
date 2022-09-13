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
* `LogicalPlanner` is requested to [buildPersistentLogicalPlan](planner/LogicalPlanner.md#buildPersistentLogicalPlan)
* `FlatMapNode` is [created](planner/FlatMapNode.md#tableFunctions) and [buildSchema](planner/FlatMapNode.md#buildSchema)

### <span id="getInto"> getInto

```java
Optional<Into> getInto()
```

See [Analysis](Analysis.md#getInto)

Used when:

* `PullQueryValidator` is requested to [validate](PullQueryValidator.md#validate)
* `PushQueryValidator` is requested to `validate`
* `LogicalPlanner` is requested to [buildOutputNode](planner/LogicalPlanner.md#buildOutputNode), [getTargetSchema](planner/LogicalPlanner.md#getTargetSchema), [buildAggregateNode](planner/LogicalPlanner.md#buildAggregateNode), [buildUserProjectNode](planner/LogicalPlanner.md#buildUserProjectNode) and [buildAggregateSchema](planner/LogicalPlanner.md#buildAggregateSchema)

### <span id="getJoin"> getJoin

```java
List<JoinInfo> getJoin()
```

See [Analysis](Analysis.md#getJoin)

Used when:

* `LogicalPlanner` is requested to [build a source node](planner/LogicalPlanner.md#buildSourceNode)

### <span id="getSelectItems"> getSelectItems

```java
List<SelectItem> getSelectItems()
```

See [Analysis](Analysis.md#getSelectItems)

Used when:

* `Analyzer.Visitor` is requested to [throwOnUnknownColumnReference](Analyzer_Visitor.md#throwOnUnknownColumnReference)
* `PullQueryValidator` is requested to [disallowedColumnNameInSelectClause](PullQueryValidator.md#disallowedColumnNameInSelectClause)
* `LogicalPlanner` is requested to [buildQueryLogicalPlan](planner/LogicalPlanner.md#buildQueryLogicalPlan), [buildAggregateNode](planner/LogicalPlanner.md#buildAggregateNode), [buildUserProjectNode](planner/LogicalPlanner.md#buildUserProjectNode), [buildJoinKey](planner/LogicalPlanner.md#buildJoinKey)
* `FlatMapNode` is requested to [buildColumnMappings](planner/FlatMapNode.md#buildColumnMappings)

### <span id="isJoin"> isJoin

```java
boolean isJoin()
```

See [Analysis](Analysis.md#isJoin)

Used when:

* `PullQueryValidator` is [created](PullQueryValidator.md#RULES)
* `LogicalPlanner` is requested to [build a source node](planner/LogicalPlanner.md#buildSourceNode)

## Implementations

* [Analysis](Analysis.md)
