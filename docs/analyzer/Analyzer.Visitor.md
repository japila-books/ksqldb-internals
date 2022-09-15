# Analyzer.Visitor

`Analyzer.Visitor` is a [DefaultTraversalVisitor](../parser/DefaultTraversalVisitor.md) to produce an `AstNode` for [Analyzer](Analyzer.md) (to [analyze a KSQL query statement](Analyzer.md#analyze)).

`Analyzer.Visitor` is a `private final class` of [Analyzer](Analyzer.md).

## Creating Instance

`Analyzer.Visitor` takes the following to be created:

* <span id="query"> [Query](../parser/Query.md)
* [persistent](#persistent) flag

`Analyzer.Visitor` is created when:

* `Analyzer` is requested to [analyze](Analyzer.md#analyze)

### <span id="analysis"> Analysis

`Analyzer.Visitor` creates an [Analysis](Analysis.md) when [created](#creating-instance).

The `Analysis` instance is mutated (_changed_) while visiting AST nodes (for [Analyzer](Analyzer.md) to [analyze a KSQL query statement](Analyzer.md#analyze)).

### <span id="persistent"> persistent Flag

`Analyzer.Visitor` is given `persistent` flag when [created](#creating-instance).

`persistent` flag indicates whether the [Query](#query) has got a [Sink](../parser/Sink.md) or not.

In other words, a query is persistent when has got [Sink](../parser/Sink.md) defined.

## <span id="analyzeNonStdOutSink"> analyzeNonStdOutSink

```java
void analyzeNonStdOutSink(
  Sink sink)
```

`analyzeNonStdOutSink`...FIXME

---

`analyzeNonStdOutSink` is used when:

* `Analyzer` is requested to [analyze a Query statement](Analyzer.md#analyze) (with a [Sink](../parser/Sink.md))

## <span id="visitAliasedRelation"> visitAliasedRelation

```java
AstNode visitAliasedRelation(
  AliasedRelation node,
  Void context)
```

`visitAliasedRelation` is part of the [AstVisitor](../parser/AstVisitor.md#visitAliasedRelation) abstraction.

---

`visitAliasedRelation` makes sure that the `Table` relation is registered in the [MetaStore](Analyzer.md#metaStore) and requests the [Analysis](#analysis) to [register the alias with the DataSource](Analysis.md#addDataSource).

## <span id="visitSelect"> visitSelect

```java
AstNode visitSelect(
  Select node,
  Void context)
```

`visitSelect` is part of the [AstVisitor](../parser/AstVisitor.md#visitSelect) abstraction.

---

`visitSelect`...FIXME

### <span id="visitTableFunctions"> visitTableFunctions

```java
void visitTableFunctions(
  Expression expression)
```

`visitTableFunctions` creates a `TableFunctionVisitor` to `process` the given [Expression](../parser/Expression.md).
