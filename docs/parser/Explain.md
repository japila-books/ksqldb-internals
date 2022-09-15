# Explain

`Explain` is a [Statement](Statement.md).

## Creating Instance

`Explain` takes the following to be created:

* <span id="location"> Node Location (in a SQL text)
* <span id="queryId"> Query ID
* <span id="statement"> [Statement](Statement.md)

`Explain` is created when:

* `AstBuilder.Visitor` is requested to [parse EXPLAIN statement](AstBuilder.Visitor.md#visitExplain)
* `StatementRewriter.Rewriter` is requested to `visitExplain`

## <span id="accept"> accept

```java
R accept(
  AstVisitor<R, C> visitor,
  C context)
```

`accept` requests the given [AstVisitor](AstVisitor.md) to [visitExplain](AstVisitor.md#visitExplain).

`accept` is part of the [Statement](Statement.md#accept) abstraction.
