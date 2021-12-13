# Visitor

`Visitor` is a `SqlBaseBaseVisitor` to build a [Node](Node.md).

!!! danger "ANTLR"
    `SqlBaseBaseVisitor` is generated from `SqlBase.g4` SQL grammar by ANTLR at build time.

## Creating Instance

`Visitor` takes the following to be created:

* <span id="sources"> Source Names
* <span id="typeRegistry"> `TypeRegistry`

`Visitor` is created when:

* `AstBuilder` is requested to [build a node tree](AstBuilder.md#build)

## <span id="visitQuery"> visitQuery

```java
Query visitQuery(
  SqlBaseParser.QueryContext context)
```

`visitQuery` is part of the `SqlBaseBaseVisitor` abstraction to handle `SELECT` statements (queries).

```antlr
query
    : SELECT selectItem (',' selectItem)*
      FROM from=relation
      (WINDOW  windowExpression)?
      (WHERE where=booleanExpression)?
      (GROUP BY groupBy)?
      (PARTITION BY partitionBy)?
      (HAVING having=booleanExpression)?
      (EMIT resultMaterialization)?
      limitClause?
    ;
```

`visitQuery`...FIXME

## <span id="visitInsertInto"> Parsing INSERT INTO Statement

```java
Node visitInsertInto(
  SqlBaseParser.InsertIntoContext context)
```

`visitInsertInto` is part of the `SqlBaseBaseVisitor` abstraction to handle `INSERT INTO` statements.

```antlr
INSERT INTO sourceName (WITH tableProperties)? query
```

`visitInsertInto` creates an [InsertInto](InsertInto.md).
