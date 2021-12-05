# DefaultKsqlParser

`DefaultKsqlParser` is a [KsqlParser](KsqlParser.md).

## Creating Instance

`DefaultKsqlParser` takes no arguments to be created.

`DefaultKsqlParser` is created when:

* `Cli` is [created](cli/Cli.md#KSQL_PARSER)
* `EngineContext` utility is used to [create an EngineContext](EngineContext.md#create) and [createSandbox](EngineContext.md#createSandbox)
* `KsqlResource` is requested for [TERMINATE_CLUSTER](rest/KsqlResource.md#TERMINATE_CLUSTER)

## <span id="parse"> parse

```java
List<ParsedStatement> parse(
  String sql)
```

`parse`...FIXME

`parse` is part of the [KsqlParser](KsqlParser.md#parse) abstraction.
