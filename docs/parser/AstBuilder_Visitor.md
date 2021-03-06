# AstBuilder.Visitor

`Visitor` is a `SqlBaseBaseVisitor` to build a [Node](Node.md) tree (that `AstBuilder` uses to [build a parsed tree](AstBuilder.md#build)).

!!! warning "ANTLR"
    `SqlBaseBaseVisitor` is generated from `SqlBase.g4` SQL grammar by ANTLR at build time.

## Creating Instance

`Visitor` takes the following to be created:

* <span id="sources"> Source Names
* <span id="typeRegistry"> `TypeRegistry`

`Visitor` is created when:

* `AstBuilder` is requested to [build a parsed tree](AstBuilder.md#build)

## Parsing Statements

### <span id="visitAssertTable"> ASSERT TABLE

```java
Node visitAssertTable(
  AssertTableContext context)
```

`visitAssertTable` is part of the `SqlBaseBaseVisitor` abstraction to handle `ASSERT TABLE` statements:

```antlr
ASSERT TABLE sourceName
(tableElements)?
(WITH tableProperties)?
```

`visitAssertTable` creates an `AssertTable` with a [CreateTable](CreateTable.md).

### <span id="visitCreateStream"> CREATE STREAM

```java
Node visitCreateStream(
  SqlBaseParser.CreateStreamContext context)
```

`visitCreateStream` is part of the `SqlBaseBaseVisitor` abstraction to handle `CREATE STREAM` statements:

```antlr
CREATE (OR REPLACE)? (SOURCE)? STREAM (IF NOT EXISTS)? sourceName
(tableElements)?
(WITH tableProperties)?

tableElements
    : '(' tableElement (',' tableElement)* ')'
    ;

tableProperties
    : '(' tableProperty (',' tableProperty)* ')'
    ;
```

`visitCreateStream` creates an [CreateStream](CreateStream.md).

### <span id="visitCreateTable"> CREATE TABLE

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

`visitCreateTable` creates a [CreateTable](CreateTable.md).

### <span id="visitExplain"> EXPLAIN

```java
Node visitExplain(
  SqlBaseParser.ExplainContext ctx)
```

`visitExplain` is part of the `SqlBaseBaseVisitor` abstraction to handle `EXPLAIN` statements.

```antlr
EXPLAIN (statement | identifier)
```

`visitExplain` creates an [Explain](Explain.md).

### <span id="visitInsertInto"> INSERT INTO

```java
Node visitInsertInto(
  SqlBaseParser.InsertIntoContext context)
```

`visitInsertInto` is part of the `SqlBaseBaseVisitor` abstraction to handle `INSERT INTO` statements.

```antlr
INSERT INTO sourceName (WITH tableProperties)? query
```

`visitInsertInto` creates an [InsertInto](InsertInto.md).

### <span id="visitCreateStreamAs"> visitCreateStreamAs

```java
Node visitCreateStreamAs(
  SqlBaseParser.CreateStreamAsContext context)
```

`visitCreateStreamAs` is part of the `SqlBaseBaseVisitor` abstraction to handle `CREATE STREAM AS` (CSAS) statements.

```antlr
CREATE (OR REPLACE)? STREAM (IF NOT EXISTS)? sourceName
  (WITH tableProperties)? AS query
```

`visitCreateStreamAs` creates a [CreateStreamAsSelect](CreateStreamAsSelect.md).

### <span id="visitCreateTableAs"> visitCreateTableAs

```java
Node visitCreateTableAs(
  SqlBaseParser.CreateTableAsContext context)
```

`visitCreateTableAs` is part of the `SqlBaseBaseVisitor` abstraction to handle `CREATE TABLE AS` (CTAS) statements.

```antlr
CREATE (OR REPLACE)? TABLE (IF NOT EXISTS)? sourceName
  (WITH tableProperties)? AS query
```

`visitCreateTableAs` creates a [CreateTableAsSelect](CreateTableAsSelect.md).

### <span id="visitInsertInto"> visitInsertInto

```java
Node visitInsertInto(
  SqlBaseParser.InsertIntoContext context)
```

`visitInsertInto` is part of the `SqlBaseBaseVisitor` abstraction to handle `INSERT INTO` statements.

```antlr
INSERT INTO sourceName (WITH tableProperties)? query
```

`visitInsertInto` creates a [InsertInto](InsertInto.md).

### <span id="visitQuery"> SELECT

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

`visitQuery` creates a [Query](Query.md).

## <span id="buildingPersistentQuery"> buildingPersistentQuery Flag

`Visitor` defines `buildingPersistentQuery` internal flag that is `false` when [created](#creating-instance).

`buildingPersistentQuery` flag is turned on (`true`) when [withinPersistentQuery](#withinPersistentQuery).

## <span id="withinPersistentQuery"> withinPersistentQuery

```java
T withinPersistentQuery(
  Supplier<T> task)
```

`withinPersistentQuery`...FIXME

`withinPersistentQuery` is used when:

* `Visitor` is requested to parse [CreateStreamAs](#visitCreateStreamAs), [CreateTableAs](#visitCreateTableAs) and [InsertInto](#visitInsertInto) statements
