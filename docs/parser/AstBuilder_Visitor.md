# AstBuilder.Visitor

`Visitor` is a `SqlBaseBaseVisitor` to build a [Node](Node.md) (that `AstBuilder` uses to [build a parsed tree](AstBuilder.md#build)).

!!! warning "ANTLR"
    `SqlBaseBaseVisitor` is generated from `SqlBase.g4` SQL grammar by ANTLR at build time.

## Creating Instance

`Visitor` takes the following to be created:

* <span id="sources"> Source Names
* <span id="typeRegistry"> `TypeRegistry`

`Visitor` is created when:

* `AstBuilder` is requested to [build a parsed tree](AstBuilder.md#build)

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

## <span id="visitCreateStream"> Parsing CREATE STREAM Statement (visitCreateStream)

```java
Node visitCreateStream(
  SqlBaseParser.CreateStreamContext context)
```

`visitCreateStream` is part of the `SqlBaseBaseVisitor` abstraction to handle `CREATE STREAM` statements.

```antlr
CREATE (OR REPLACE)? (SOURCE)? STREAM (IF NOT EXISTS)? sourceName
(tableElements)?
(WITH tableProperties)?
```

`visitCreateStream` creates an [CreateStream](CreateStream.md).

## <span id="visitCreateTable"> Parsing CREATE TABLE Statement (visitCreateTable)

```java
Node visitCreateTable(
  SqlBaseParser.CreateTableContext context)
```

`visitCreateTable` is part of the `SqlBaseBaseVisitor` abstraction to handle `CREATE TABLE` statements.

```antlr
CREATE (OR REPLACE)? (SOURCE)? TABLE (IF NOT EXISTS)? sourceName
(tableElements)?
(WITH tableProperties)?
```

`visitCreateTable` creates an [CreateTable](CreateTable.md).

## <span id="visitExplain"> Parsing EXPLAIN Statement (visitExplain)

```java
Node visitExplain(
  SqlBaseParser.ExplainContext ctx)
```

`visitExplain` is part of the `SqlBaseBaseVisitor` abstraction to handle `EXPLAIN` statements.

```antlr
EXPLAIN (statement | identifier)
```

`visitExplain` creates an [Explain](Explain.md) node.

## <span id="visitInsertInto"> Parsing INSERT INTO Statement (visitInsertInto)

```java
Node visitInsertInto(
  SqlBaseParser.InsertIntoContext context)
```

`visitInsertInto` is part of the `SqlBaseBaseVisitor` abstraction to handle `INSERT INTO` statements.

```antlr
INSERT INTO sourceName (WITH tableProperties)? query
```

`visitInsertInto` creates an [InsertInto](InsertInto.md).
