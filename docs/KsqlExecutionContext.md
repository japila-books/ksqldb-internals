# KsqlExecutionContext

`KsqlExecutionContext` is an [abstraction](#contract) of [execution contexts](#implementations).

## Contract (Subset)

### <span id="getAllLiveQueries"> getAllLiveQueries

```java
List<QueryMetadata> getAllLiveQueries()
```

Used when:

* `ListQueriesExecutor` is requested to `getLocalSimple`, `getLocalExtended`
* `QueryCapacityUtil` utility is used to `getNumLivePushQueries`

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

### <span id="prepare"> Preparing ParsedStatement

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
PreparedStatement<?> prepare(
  ParsedStatement stmt) // (1)
```

1. Uses an empty `Map`

Used when:

* `KsqlContext` is requested to [execute](embedded/KsqlContext.md#execute)
* `KsqlRestApplication` is requested to [registerCommandTopic](rest/KsqlRestApplication.md#registerCommandTopic)
* `QueryEndpoint` is requested to `createStatement`
* `RequestHandler` is requested to [execute](rest/RequestHandler.md#execute)
* `RequestValidator` is requested to `validate`
* `SqlFormatInjector` is requested to `inject`
* `StandaloneExecutor.StatementExecutor` is requested to `prepare` a `ParsedStatement`
* `StatementParser` is requested to `parseSingleStatement`

## Implementations

* [KsqlEngine](KsqlEngine.md)
* [SandboxedExecutionContext](SandboxedExecutionContext.md)
