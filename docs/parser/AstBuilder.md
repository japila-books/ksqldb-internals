# AstBuilder

`AstBuilder` uses [Visitor](AstBuilder_Visitor.md) to parse SQL statements (using ANTLR).

## Creating Instance

`AstBuilder` takes the following to be created:

* <span id="typeRegistry"> [TypeRegistry](../TypeRegistry.md)

`AstBuilder` is created when:

* `QueryAnonymizer.Visitor` is requested to `visitExpression`
* `DefaultKsqlParser` is requested to [prepare](DefaultKsqlParser.md#prepare)
* `ExpressionParser` is requested to `parseSelectExpression`, `parseExpression`, `parseWindowExpression`

## <span id="buildStatement"> Building Statement

```java
Statement buildStatement(
  ParserRuleContext parseTree)
```

`buildStatement` [collects the source names](#getSources) (in a SQL statement) and [builds a Statement](#build).

---

`buildStatement` is used when:

* `DefaultKsqlParser` is requested to [prepare a ParsedStatement](DefaultKsqlParser.md#prepare)

### <span id="getSources"> Collecting Source Names

```java
Set<SourceName> getSources(
  ParseTree parseTree)
```

`getSources` creates a [SourceAccumulator](SourceAccumulator.md) to visit (the nodes of) the given `ParseTree`. In the end, `getSources` requests the `SourceAccumulator` for the [sources](SourceAccumulator.md#getSources).

## <span id="build"> Building Parsed Tree (build)

```java
<T extends Node> T build(
  Optional<Set<SourceName>> sources, // (1)!
  ParserRuleContext parseTree)
```

1. Only given when `AstBuilder` is requested to build a [Statement](#buildStatement)

`build` creates a [Visitor](AstBuilder_Visitor.md) to build a [node tree](Node.md) (for a given `ParserRuleContext` that represents a parsed SQL text).

---

`build` is used when:

* `AstBuilder` is requested to build a [Statement](#buildStatement), an [Expression](#buildExpression), a [WindowExpression](#buildWindowExpression) and an [AssertStatement](#buildAssertStatement)
