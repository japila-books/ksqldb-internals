# KsqlExecutionContext

`KsqlExecutionContext` is an [abstraction](#contract) of [execution contexts](#implementations).

## Contract (Subset)

### <span id="parse"> Parsing SQL Text

```java
List<ParsedStatement> parse(
  String sql)
```

Used when:

* `KsqlContext` is requested to [sql](embedded/KsqlContext.md#sql)
* `SqlFormatInjector` is requested to `inject`
* `QueryEndpoint` is requested to `createStatement`
* `KsqlRestApplication` is requested to [registerCommandTopic](rest/KsqlRestApplication.md#registerCommandTopic)
* `StandaloneExecutor` is requested to [processesQueryFile](rest/StandaloneExecutor.md#processesQueryFile)
* `StatementParser` is requested to `parseSingleStatement`
* `KsqlResource` is requested to [handleKsqlStatements](rest/KsqlResource.md#handleKsqlStatements)

## Implementations

* [KsqlEngine](KsqlEngine.md)
* [SandboxedExecutionContext](SandboxedExecutionContext.md)
