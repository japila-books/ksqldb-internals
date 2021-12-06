# AstBuilder

## Creating Instance

`AstBuilder` takes the following to be created:

* <span id="typeRegistry"> `TypeRegistry`

`AstBuilder` is created when:

* `QueryAnonymizer.Visitor` is requested to `visitExpression`
* `DefaultKsqlParser` is requested to [prepare](DefaultKsqlParser.md#prepare)
* `ExpressionParser` is requested to `parseSelectExpression`, `parseExpression`, `parseWindowExpression`

## <span id="buildStatement"> buildStatement

```java
Statement buildStatement(
  ParserRuleContext parseTree)
```

`buildStatement`...FIXME

`buildStatement` is used when:

* `DefaultKsqlParser` is requested to [prepare a ParsedStatement](DefaultKsqlParser.md#prepare)

## <span id="build"> build

```java
<T extends Node> T build(
  Optional<Set<SourceName>> sources,
  ParserRuleContext parseTree)
```

`build` creates a `Visitor` and requests it to `visit` the `parseTree` (and create a `Node`).

`build` is used when:

* `AstBuilder` is requested to [buildStatement](#buildStatement), [buildExpression](#buildExpression), [buildWindowExpression](#buildWindowExpression) and [buildAssertStatement](#buildAssertStatement)
