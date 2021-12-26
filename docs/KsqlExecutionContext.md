# KsqlExecutionContext

`KsqlExecutionContext` is an [abstraction](#contract) of [execution contexts](#implementations).

## Contract (Subset)

### <span id="createSandbox"> createSandbox

```java
KsqlExecutionContext createSandbox(
  ServiceContext serviceContext)
```

Used when:

* `KsqlContext` is requested to [execute a SQL text](embedded/KsqlContext.md#sql)
* `DefaultSchemaInjector` is requested to `forCreateAsStatement`
* `SchemaRegisterInjector` is requested to `registerForCreateAs`
* `StandaloneExecutor` is requested to [validateStatements](rest/StandaloneExecutor.md#validateStatements)
* `DistributingExecutor` is requested to [execute](rest/DistributingExecutor.md#execute)
* `ExplainExecutor` is requested to `explainStatement`
* `KsqlResource` is requested to [configure](rest/KsqlResource.md#configure)

### <span id="executeTablePullQuery"> executeTablePullQuery

```java
PullQueryResult executeTablePullQuery(
  ImmutableAnalysis analysis,
  ServiceContext serviceContext,
  ConfiguredStatement<Query> statement,
  HARouting routing,
  RoutingOptions routingOptions,
  QueryPlannerOptions queryPlannerOptions,
  Optional<PullQueryExecutorMetrics> pullQueryMetrics,
  boolean startImmediately,
  Optional<ConsistencyOffsetVector> consistencyOffsetVector)
```

Used when:

* `QueryExecutor` is requested to [handleTablePullQuery](rest/QueryExecutor.md#handleTablePullQuery)

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

### <span id="plan"> Query Planning

```java
KsqlPlan plan(
  ServiceContext serviceContext,
  ConfiguredStatement<?> statement)
```

Used when:

* `KsqlEngine` is requested to [execute a SQL statement](KsqlEngine.md#execute)
* `SandboxedExecutionContext` is requested to [execute a SQL statement](SandboxedExecutionContext.md#execute)
* `SchemaRegisterInjector` is requested to `registerForCreateAs`
* `ValidatedCommandFactory` is requested to [createForPlannedQuery](rest/ValidatedCommandFactory.md#createForPlannedQuery)

### <span id="prepare"> Preparing Statement for Execution

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
* `RequestValidator` is requested to [validate](rest/RequestValidator.md#validate)
* `SqlFormatInjector` is requested to `inject`
* `StandaloneExecutor.StatementExecutor` is requested to [prepare a statement](rest/StandaloneExecutor_StatementExecutor.md#prepare)
* `StatementParser` is requested to `parseSingleStatement`

## Implementations

* [KsqlEngine](KsqlEngine.md)
* [SandboxedExecutionContext](SandboxedExecutionContext.md)
