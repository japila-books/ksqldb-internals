# AstBuilder

`AstBuilder` uses [Visitor](Visitor.md) to parse SQL statements (using ANTLR).

## Creating Instance

`AstBuilder` takes the following to be created:

* <span id="typeRegistry"> `TypeRegistry`

`AstBuilder` is created when:

* `QueryAnonymizer.Visitor` is requested to `visitExpression`
* `DefaultKsqlParser` is requested to [prepare](DefaultKsqlParser.md#prepare)
* `ExpressionParser` is requested to `parseSelectExpression`, `parseExpression`, `parseWindowExpression`

## <span id="buildStatement"> Building Statement

```java
Statement buildStatement(
  ParserRuleContext parseTree)
```

`buildStatement`...FIXME

`buildStatement` is used when:

* `DefaultKsqlParser` is requested to [prepare a ParsedStatement](DefaultKsqlParser.md#prepare)

## <span id="build"> Parsing SQL Text

```java
<T extends Node> T build(
  Optional<Set<SourceName>> sources, // (1)!
  ParserRuleContext parseTree)
```

1. Only given when `AstBuilder` is requested to build a [Statement](#buildStatement)

`build` creates a [Visitor](AstBuilder_Visitor.md) to build a [node tree](Node.md) (for a given `ParserRuleContext` that represents a parsed SQL text).

`build` is used when:

* `AstBuilder` is requested to build a [Statement](#buildStatement), an [Expression](#buildExpression), a [WindowExpression](#buildWindowExpression) and an [AssertStatement](#buildAssertStatement)
