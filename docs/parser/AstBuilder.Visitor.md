# AstBuilder.Visitor

`Visitor` is a `SqlBaseBaseVisitor` to build a [Node](Node.md) tree (that `AstBuilder` uses to [build a parsed tree](AstBuilder.md#build)).

!!! warning "ANTLR"
    `SqlBaseBaseVisitor` is generated from [SqlBase.g4]({{ ksqldb.github }}/ksqldb-parser/src/main/antlr4/io/confluent/ksql/parser/SqlBase.g4) SQL grammar by ANTLR at build time.

## Creating Instance

`Visitor` takes the following to be created:

* <span id="sources"> Source Names
* <span id="typeRegistry"> [TypeRegistry](../TypeRegistry.md)

`Visitor` is created when:

* `AstBuilder` is requested to [build a parsed tree](AstBuilder.md#build)

## Parsing Statements

### <span id="visitAlterSource"> ALTER SOURCE

```java
Node visitAlterSource(
  AlterSourceContext ctx)
```

`visitAlterSource` is part of the `SqlBaseBaseVisitor` abstraction to handle `ALTER SYSTEM` statements.

```antlr
ALTER (STREAM | TABLE) sourceName alterOption (',' alterOption)*

alterOption
    : ADD (COLUMN)? identifier type
    ;
```

`visitAlterSource` creates an [AlterSource](AlterSource.md).

### <span id="visitAlterSystemProperty"> ALTER SYSTEM

```java
Node visitAlterSystemProperty(
  SqlBaseParser.AlterSystemPropertyContext context)
```

`visitAlterSystemProperty` is part of the `SqlBaseBaseVisitor` abstraction to handle `ALTER SYSTEM` statements.

```antlr
ALTER SYSTEM name = value   #alterSystemProperty
```

`visitAlterSystemProperty` creates an [AlterSystemProperty](AlterSystemProperty.md).

!!! warning "Not supported"
    Not supported with [ksql.runtime.feature.shared.enabled](../KsqlConfig.md#ksql.runtime.feature.shared.enabled) turned off ([createForAlterSystemQuery](../rest/ValidatedCommandFactory.md#createForAlterSystemQuery)).

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

`visitCreateStream` is part of the `SqlBaseBaseVisitor` abstraction to handle `CREATE STREAM` statements.

```antlr
CREATE (OR REPLACE)? (SOURCE)? STREAM (IF NOT EXISTS)? sourceName
(tableElements)?
(WITH tableProperties)?

tableElements
    : '(' tableElement (',' tableElement)* ')'
    ;

tableElement
    : identifier type columnConstraints?
    ;

type
    : type ARRAY
    | ARRAY '<' type '>'
    | MAP '<' type ',' type '>'
    | STRUCT '<' (identifier type (',' identifier type)*)? '>'
    | DECIMAL '(' number ',' number ')'
    | baseType ('(' typeParameter (',' typeParameter)* ')')?
    ;

columnConstraints
    : ((PRIMARY)? KEY)
    | HEADERS
    | HEADER '(' text ')'
    ;

tableProperties
    : '(' tableProperty (',' tableProperty)* ')'
    ;
```

`visitCreateStream` creates a [CreateStream](CreateStream.md).

### <span id="visitCreateStreamAs"> CREATE STREAM AS SELECT

```java
Node visitCreateStreamAs(
  SqlBaseParser.CreateStreamAsContext context)
```

`visitCreateStreamAs` is part of the `SqlBaseBaseVisitor` abstraction to handle `CREATE STREAM AS` (CSAS) statements.

```antlr
CREATE (OR REPLACE)? STREAM (IF NOT EXISTS)? sourceName
  (WITH tableProperties)?
  AS query
```

`visitCreateStreamAs` creates a [CreateStreamAsSelect](CreateStreamAsSelect.md).

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

tableElements
    : '(' tableElement (',' tableElement)* ')'
    ;

tableElement
    : identifier type columnConstraints?
    ;

columnConstraints
    : ((PRIMARY)? KEY)
    | HEADERS
    | HEADER '(' STRING ')'
    ;
```

`visitCreateTable` creates a [CreateTable](CreateTable.md).

### <span id="visitCreateTableAs"> CREATE TABLE AS SELECT

```java
Node visitCreateTableAs(
  SqlBaseParser.CreateTableAsContext context)
```

`visitCreateTableAs` is part of the `SqlBaseBaseVisitor` abstraction to handle `CREATE TABLE AS SELECT` (CTAS) statements.

```antlr
CREATE (OR REPLACE)? TABLE (IF NOT EXISTS)? sourceName
  (WITH tableProperties)?
  AS query
```

`visitCreateTableAs` creates a [CreateTableAsSelect](CreateTableAsSelect.md).

### <span id="visitShowColumns"> DESCRIBE SOURCE

`visitShowColumns` handles `DESCRIBE [source]` statements.

```antlr
DESCRIBE sourceName EXTENDED?
```

`visitShowColumns` creates the following:

* [DescribeTables](DescribeTables.md) for `DESCRIBE TABLES` (when the `sourceName` is `TABLES` case-insensitive)
* [ShowColumns](ShowColumns.md) otherwise

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

`visitInsertInto` handles `INSERT INTO` statements.

```antlr
INSERT INTO sourceName
  (WITH tableProperties)?
  query
```

`visitInsertInto` creates an [InsertInto](InsertInto.md).

### <span id="visitListStreams"> LIST STREAMS

```java
Node visitListStreams(
  SqlBaseParser.ListStreamsContext context)
```

`visitListStreams` is part of the `SqlBaseBaseVisitor` abstraction to handle `LIST STREAMS` statements.

```antlr
(LIST | SHOW) STREAMS EXTENDED?
```

`visitListStreams` creates a [ListStreams](ListStreams.md).

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
