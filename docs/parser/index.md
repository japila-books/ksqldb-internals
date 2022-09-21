# Statement Parsing

!!! note "Title"
    A title "Query Parsing" would probably make more sense but [Query](Query.md) is an overloaded term in ksqlDB (a subtype of a [Statement](Statement.md) which is more generic term) so I decided to use the latter instead.

ksqlDB uses [AstBuilder](AstBuilder.md) (and [AstBuilder.Visitor](AstBuilder.Visitor.md)) to parse and [build a tree node](AstBuilder.md#build) for a KSQL text:

* [AssertStatement](AssertStatement.md)
* [Expression](Expression.md)
* [Statement](Statement.md)
* [WindowExpression](WindowExpression.md)

ksqlDB uses [DefaultKsqlParser](DefaultKsqlParser.md) to [prepare a statement for execution](DefaultKsqlParser.md#prepare) (using [AstBuilder](AstBuilder.md) with [AstBuilder.Visitor](AstBuilder.Visitor.md)).

In the end, ksqlDB uses [EngineContext](../EngineContext.md) for [Variable Substitution](../EngineContext.md#substituteVariables) and [Sanitization](../AstSanitizer.md#sanitize) while [preparing a KSQL statement for execution](../EngineContext.md#prepare).

## Statements

KSQL | Statement
-----|----------
 [DESCRIBE SOURCE](AstBuilder.Visitor.md#visitShowColumns) | [ShowColumns](ShowColumns.md)
 _others_ |
