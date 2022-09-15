# AstVisitor

`AstVisitor<R, C>` is an abstraction of [AST visitors](#implementations).

## Implementations

* `AstSanitizer.RewriterPlugin`
* [DefaultTraversalVisitor](DefaultTraversalVisitor.md)
* `ScalablePushUtil.SourceFinder`
* `SqlFormatter.Formatter`
* `StatementRewriter.Rewriter`

## Creating Instance

`AstVisitor` takes the following to be created:

* [Default Result](#defaultResult)

!!! note "Abstract Class"
    `AstVisitor` is an abstract class and cannot be created directly. It is created indirectly for the [concrete AstVisitors](#implementations).

### <span id="defaultResult"> Default Result

```java
R defaultResult
```

`AstVisitor` can be given `defaultResult` value when [created](#creating-instance). Unless given, `defaultResult` value is undefined (`null`).

`defaultResult` is the default result of [visitNode](#visitNode).

## <span id="process"> process

```java
R process(
  AstNode node,
  C context)
```

`process` requests the given `AstNode` to `accept` this `AstVisitor` (and the `context`).

## <span id="visitQuery"> visitQuery

```java
R visitQuery(
  Query node,
  C context)
```

`visitQuery` [visitStatement](#visitStatement).

---

`visitQuery` is used when:

* `Query` is requested to [accept](Query.md#accept)

## <span id="visitStatement"> visitStatement

```java
R visitStatement(
  Statement node,
  C context)
```

`visitStatement` [visitNode](#visitNode).

## <span id="visitNode"> visitNode

```java
R visitNode(
  AstNode node,
  C context)
```

`visitNode` returns the [defaultResult](#defaultResult).
