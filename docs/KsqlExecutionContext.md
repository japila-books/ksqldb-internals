# KsqlExecutionContext

`KsqlExecutionContext` is an [abstraction](#contract) of [execution contexts](#implementations).

## Contract (Subset)

### <span id="createStreamPullQuery"> Creating Stream Pull Query

```java
StreamPullQueryMetadata createStreamPullQuery(
  ServiceContext serviceContext,
  ImmutableAnalysis analysis,
  ConfiguredStatement<Query> statementOrig,
  boolean excludeTombstones)
```

See [KsqlEngine](KsqlEngine.md#createStreamPullQuery)

Used when:

* `QueryExecutor` is requested to [handle a stream pull query](rest/QueryExecutor.md#handleStreamPullQuery)

### <span id="createSandbox"> Creating Sandboxed Execution Context (createSandbox)

```java
KsqlExecutionContext createSandbox(
  ServiceContext serviceContext)
```

Used when:

* `KsqlContext` is requested to [execute a SQL text](embedded/KsqlContext.md#sql)
* `DefaultSchemaInjector` is requested to `forCreateAsStatement`
* `SchemaRegisterInjector` is requested to [registerForCreateAs](SchemaRegisterInjector.md#registerForCreateAs)
* `StandaloneExecutor` is requested to [validateStatements](headless/StandaloneExecutor.md#validateStatements)
* `DistributingExecutor` is requested to [execute](rest/DistributingExecutor.md#execute)
* `ExplainExecutor` is requested to `explainStatement`
* `KsqlResource` is requested to [configure](rest/KsqlResource.md#configure)

### <span id="execute"> Statement Execution (execute)

```java
ExecuteResult execute(
  ServiceContext serviceContext,
  ConfiguredKsqlPlan plan)
ExecuteResult execute(
  ServiceContext serviceContext,
  ConfiguredStatement<?> statement)
```

Executes a [KsqlPlan](KsqlPlan.md) or a [Statement](parser/Statement.md)

Used when:

* `InteractiveStatementExecutor` is requested to [execute a KsqlPlan](rest/InteractiveStatementExecutor.md#executePlan)
* `KsqlContext` is requested to [execute a statement](embedded/KsqlContext.md#execute)
* `StandaloneExecutor.StatementExecutor` is requested to [handleExecutableDdl](headless/StandaloneExecutor_StatementExecutor.md#handleExecutableDdl) and [handlePersistentQuery](headless/StandaloneExecutor_StatementExecutor.md#handlePersistentQuery)
* `ExplainExecutor` is requested to [explain a Statement](rest/ExplainExecutor.md#explainStatement)
* `KsqlRestApplication` is requested to [registerCommandTopic](rest/KsqlRestApplication.md#registerCommandTopic)
* `ValidatedCommandFactory` is requested to [createForPlannedQuery](rest/ValidatedCommandFactory.md#createForPlannedQuery)

### <span id="executeScalablePushQuery"> Executing Scalable Push Query

```java
ScalablePushQueryMetadata executeScalablePushQuery(
  ImmutableAnalysis analysis,
  ServiceContext serviceContext,
  ConfiguredStatement<Query> statement,
  PushRouting pushRouting,
  PushRoutingOptions pushRoutingOptions,
  QueryPlannerOptions queryPlannerOptions,
  Context context,
  Optional<ScalablePushQueryMetrics> scalablePushQueryMetrics)
```

Used when:

* `QueryExecutor` is requested to [handle a scalable push query](rest/QueryExecutor.md#handleScalablePushQuery)

### <span id="executeTablePullQuery"> Executing Table Pull Query

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

### <span id="executeTransientQuery"> Executing Transient Query

```java
TransientQueryMetadata executeTransientQuery(
  ServiceContext serviceContext,
  ConfiguredStatement<Query> statement,
  boolean excludeTombstones)
```

Used when:

* `CustomExecutors` is requested to [handle a QUERY statement](rest/CustomExecutors.md#QUERY)
* `ExplainExecutor` is requested to [explain a QUERY statement](rest/ExplainExecutor.md#explainStatement)
* `QueryExecutor` is requested to [handlePushQuery](rest/QueryExecutor.md#handlePushQuery)

### <span id="getAllLiveQueries"> getAllLiveQueries

```java
List<QueryMetadata> getAllLiveQueries()
```

Used when:

* `ListQueriesExecutor` is requested to `getLocalSimple`, `getLocalExtended`
* `QueryCapacityUtil` utility is used to `getNumLivePushQueries`

### <span id="getPersistentQueries"> getPersistentQueries

```java
List<PersistentQueryMetadata> getPersistentQueries()
```

Used when:

* `KsqlContext` is requested to [getPersistentQueries](embedded/KsqlContext.md#getPersistentQueries)
* `KsqlEngineMetrics` is requested to `configureNumPersistentQueries` and `configureGaugeForState`
* `PersistentQuerySaturationMetrics` is requested to `run`
* `DiscoverClusterService` is requested to `runOneIteration`
* `SendLagService` is requested to `runOneIteration`
* `StandaloneExecutor` is requested to [processesQueryFile](headless/StandaloneExecutor.md#processesQueryFile)
* `CommandRunner` is requested to [processPriorCommands](rest/CommandRunner.md#processPriorCommands)
* _others_

### <span id="parse"> Parsing SQL Text

```java
List<ParsedStatement> parse(
  String sql)
```

Used when:

* `KsqlContext` is requested to [sql](embedded/KsqlContext.md#sql)
* `SqlFormatInjector` is requested to `inject`
* `QueryEndpoint` is requested to [createStatement](rest/QueryEndpoint.md#createStatement)
* `KsqlRestApplication` is requested to [registerCommandTopic](rest/KsqlRestApplication.md#registerCommandTopic)
* `StandaloneExecutor` is requested to [processesQueryFile](headless/StandaloneExecutor.md#processesQueryFile)
* `StatementParser` is requested to `parseSingleStatement`
* `KsqlResource` is requested to [handleKsqlStatements](rest/KsqlResource.md#handleKsqlStatements)

### <span id="plan"> Statement Planning (plan)

```java
KsqlPlan plan(
  ServiceContext serviceContext,
  ConfiguredStatement<?> statement)
```

Plans a given DDL/DML statement (in the [ServiceContext](ServiceContext.md)) and creates a [KsqlPlan](KsqlPlan.md)

See [KsqlEngine](KsqlEngine.md#plan) or [SandboxedExecutionContext](SandboxedExecutionContext.md#plan)

Used when:

* `KsqlEngine` is requested to [execute a statement](KsqlEngine.md#execute)
* `SandboxedExecutionContext` is requested to [execute a statement](SandboxedExecutionContext.md#execute)
* `DefaultSchemaInjector` is requested to `forCreateAsStatement`
* `SchemaRegisterInjector` is requested to [registerForCreateAs](SchemaRegisterInjector.md#registerForCreateAs)
* `ValidatedCommandFactory` is requested to [createForPlannedQuery](rest/ValidatedCommandFactory.md#createForPlannedQuery)

### <span id="prepare"> Preparing Statement for Execution

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
PreparedStatement<?> prepare(
  ParsedStatement stmt) // (1)!
```

1. Uses an empty `Map`

Used when:

* `KsqlContext` is requested to [execute](embedded/KsqlContext.md#execute)
* `KsqlRestApplication` is requested to [registerCommandTopic](rest/KsqlRestApplication.md#registerCommandTopic)
* `QueryEndpoint` is requested to `createStatement`
* `RequestHandler` is requested to [execute](rest/RequestHandler.md#execute)
* `RequestValidator` is requested to [validate](rest/RequestValidator.md#validate)
* `SqlFormatInjector` is requested to `inject`
* `StandaloneExecutor.StatementExecutor` is requested to [prepare a statement](headless/StandaloneExecutor_StatementExecutor.md#prepare)
* `StatementParser` is requested to `parseSingleStatement`

### <span id="updateStreamsPropertiesAndRestartRuntime"> updateStreamsPropertiesAndRestartRuntime

```java
void updateStreamsPropertiesAndRestartRuntime()
```

Used when:

* `InteractiveStatementExecutor` is requested to [execute an ALTER SYSTEM command](rest/InteractiveStatementExecutor.md#executeStatement)

## Implementations

* [KsqlEngine](KsqlEngine.md)
* [SandboxedExecutionContext](SandboxedExecutionContext.md)
