# KsqlParser

`KsqlParser` is an [abstraction](#contract) of [SQL parsers](#implementations).

## Contract

### <span id="parse"> Parsing SQL Statements

```java
List<ParsedStatement> parse(
  String sql)
```

Parses the given SQL text into a collection of `ParsedStatement`s. There can be zero, one or more SQL statements in the given SQL text.

See [DefaultKsqlParser](DefaultKsqlParser.md#parse)

Used when:

* `Cli` is requested to [substituteVariables](../cli/Cli.md#substituteVariables) and [handleStatements](../cli/Cli.md#handleStatements)
* `EngineContext` is requested to [parse SQL statements](../EngineContext.md#parse)
* `KsqlResource` is requested for [TERMINATE_CLUSTER](../rest/KsqlResource.md#TERMINATE_CLUSTER)

### <span id="prepare"> Preparing Statement

```java
PreparedStatement<?> prepare(
  ParsedStatement statement,
  TypeRegistry typeRegistry)
```

See [DefaultKsqlParser](DefaultKsqlParser.md#prepare)

Used when:

* `EngineContext` is requested to [prepare a statement](../EngineContext.md#prepare)

## Implementations

* [DefaultKsqlParser](DefaultKsqlParser.md)
