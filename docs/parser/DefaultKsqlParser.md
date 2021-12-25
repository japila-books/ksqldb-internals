# DefaultKsqlParser

`DefaultKsqlParser` is a [KsqlParser](KsqlParser.md).

## Creating Instance

`DefaultKsqlParser` takes no arguments to be created.

`DefaultKsqlParser` is created when:

* `Cli` is [created](../cli/Cli.md#KSQL_PARSER)
* `EngineContext` utility is used to [create an EngineContext](../EngineContext.md#create) and [createSandbox](../EngineContext.md#createSandbox)
* `KsqlResource` is requested for [TERMINATE_CLUSTER](../rest/KsqlResource.md#TERMINATE_CLUSTER)

## <span id="parse"> Parsing SQL Statements

```java
List<ParsedStatement> parse(
  String sql)
```

`parse` [parses](#getParseTree) the given `sql` (using ANTLR) and creates as many `ParsedStatement`s as there are SQL statements in the `sql`.

!!! warning "ANTLR"
    This is when a SQL text is parsed (_transformed_) using ANTLR into a collection of ksqlDB's `ParsedStatement`s according to the `SqlBase.g4` grammar:

    ```antlr
    statements
        : (singleStatement)* EOF
        ;
    ```

    Every `ParsedStatement`s maps exactly to a `singleStatement`.

`parse` is part of the [KsqlParser](KsqlParser.md#parse) abstraction.

## <span id="prepare"> Preparing ParsedStatement

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  TypeRegistry typeRegistry)
```

`prepare`...FIXME

`prepare` is part of the [KsqlParser](KsqlParser.md#prepare) abstraction.
