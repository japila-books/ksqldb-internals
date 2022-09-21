# AstBuilder

`AstBuilder` uses [AstBuilder.Visitor](AstBuilder.Visitor.md) to parse SQL statements (using ANTLR).

## Creating Instance

`AstBuilder` takes the following to be created:

* <span id="typeRegistry"> [TypeRegistry](../TypeRegistry.md)

`AstBuilder` is created when:

* `QueryAnonymizer.Visitor` is requested to `visitExpression`
* `DefaultKsqlParser` is requested to [prepare a statement for execution](DefaultKsqlParser.md#prepare)
* `ExpressionParser` is requested to `parseSelectExpression`, `parseExpression`, `parseWindowExpression`

## <span id="buildStatement"> Building Statement

```java
Statement buildStatement(
  ParserRuleContext parseTree)
```

`buildStatement` [collects the source names](#getSources) (in the SQL statement as a parsed tree) first and then [builds a Statement](#build).

---

`buildStatement` is used when:

* `DefaultKsqlParser` is requested to [prepare a ParsedStatement](DefaultKsqlParser.md#prepare)

### <span id="getSources"> Collecting Source Names

```java
Set<SourceName> getSources(
  ParseTree parseTree)
```

`getSources` creates a [SourceAccumulator](SourceAccumulator.md) to visit (the nodes of) the given `ParseTree`. In the end, `getSources` requests the `SourceAccumulator` for the [sources](SourceAccumulator.md#getSources).

## <span id="build"> Building Node

```java
<T extends Node> T build(
  Optional<Set<SourceName>> sources, // (1)!
  ParserRuleContext parseTree)
```

1. `sources` are only given for [building a Statement](#buildStatement)

`build` creates an [AstBuilder.Visitor](AstBuilder.Visitor.md) to build one of the following [Node](Node.md)s (for a given `ParserRuleContext` that is a parsed SQL statement):

* [AssertStatement](AssertStatement.md) when [building an AssertStatement](#buildAssertStatement)
* [Expression](Expression.md) when [building an Expression](#buildExpression)
* [Statement](Statement.md) when [building a Statement](#buildStatement)
* [WindowExpression](WindowExpression.md) when [building a WindowExpression](#buildWindowExpression)
