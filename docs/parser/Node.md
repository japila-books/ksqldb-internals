# Node

`Node` is an abstraction of [tree nodes](#implementations) that are result of parsing SQL statements (using [AstBuilder.Visitor](AstBuilder.Visitor.md)).

## Implementations

* [AstNode](AstNode.md)
* [Expression](Expression.md)
* [KsqlWindowExpression](KsqlWindowExpression.md)

## Creating Instance

`Node` takes the following to be created:

* <span id="location"> `NodeLocation` (with line and column numbers)

!!! note "Abstract Class"
    `Node` is an abstract class and cannot be created directly. It is created indirectly for the [concrete Nodes](#implementations).
