# KsqlParser

`KsqlParser` is an [abstraction](#contract) of [SQL parsers](#implementations).

## Contract

### <span id="parse"> Parsing SQL Text

```java
List<ParsedStatement> parse(
  String sql)
```

Used when:

* `Cli` is requested to [substituteVariables](cli/Cli.md#substituteVariables) and [handleStatements](cli/Cli.md#handleStatements)
* `EngineContext` is requested to [parse a SQL statement](EngineContext.md#parse)
* `KsqlResource` is requested for [TERMINATE_CLUSTER](rest/KsqlResource.md#TERMINATE_CLUSTER)

### <span id="prepare"> Preparing ParsedStatement

```java
PreparedStatement<?> prepare(
  ParsedStatement statement,
  TypeRegistry typeRegistry)
```

Used when:

* `EngineContext` is requested to [prepare a ParsedStatement](EngineContext.md#prepare)

## Implementations

* [DefaultKsqlParser](DefaultKsqlParser.md)
