# AstNode

`AstNode` is an [extension](#contract) of the [Node](Node.md) abstraction for [AST nodes](#implementations).

## Contract

### <span id="accept"> accept

```java
R accept(
  AstVisitor<R, C> visitor,
  C context)
```

Used when:

* `AstVisitor` is requested to [process a node](AstVisitor.md#process)

## Implementations

* [AstNode](AstNode.md)
* `AlterOption`
* `AssertStatement`
* `GroupBy`
* `PartitionBy`
* `Relation`
* `Select`
* `SelectItem`
* [Statement](Statement.md)
* `Statements`
* `TableElement`
* `WindowExpression`
* `WithinExpression`
