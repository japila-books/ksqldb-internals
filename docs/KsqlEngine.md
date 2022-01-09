# KsqlEngine

`KsqlEngine` is a facade of [EngineContext](#primaryContext).

## Creating Instance

`KsqlEngine` takes the following to be created:

* <span id="serviceContext"> ServiceContext
* <span id="processingLogContext"> ProcessingLogContext
* <span id="serviceId"> Service ID
* <span id="metaStore"> MutableMetaStore
* <span id="engineMetricsFactory"> `Function<KsqlEngine, KsqlEngineMetrics>`
* <span id="queryIdGenerator"> QueryIdGenerator
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="queryEventListeners"> `QueryEventListener`s

`KsqlEngine` is created when:

* `KsqlContext` is requested to [create](embedded/KsqlContext.md#create)
* `KsqlRestApplication` is requested to [buildApplication](rest/KsqlRestApplication.md#buildApplication)
* `StandaloneExecutorFactory` is requested to [create](rest/StandaloneExecutorFactory.md#create)

## <span id="primaryContext"> EngineContext

`KsqlEngine` [creates an EngineContext](EngineContext.md#create) when [created](#creating-instance).

`KsqlEngine` is (pretty much) a facade of the `EngineContext`.

## <span id="execute"> Executing Statement

```java
ExecuteResult execute(
  ServiceContext serviceContext,
  ConfiguredStatement<?> statement) // (1)!
ExecuteResult execute(
  ServiceContext serviceContext,
  ConfiguredKsqlPlan plan)
```

1. [Plans the statement](#plan) and creates a `ConfiguredKsqlPlan` for the other `execute`

`execute` [creates an EngineExecutor](EngineExecutor.md#create) to [execute](EngineExecutor.md#execute) the [KsqlPlan](KsqlPlan.md) (of the given `ConfiguredKsqlPlan`).

`execute` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#execute) abstraction.

## <span id="plan"> Statement Planning (plan)

```java
KsqlPlan plan(
  ServiceContext serviceContext,
  ConfiguredStatement<?> statement)
```

`plan` [creates an EngineExecutor](EngineExecutor.md#create) to [plan](EngineExecutor.md#plan) the given `ConfiguredStatement`.

`plan` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#plan) abstraction.

## <span id="getAllLiveQueries"> getAllLiveQueries

```java
List<QueryMetadata> getAllLiveQueries()
```

`getAllLiveQueries`...FIXME

`getAllLiveQueries` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#getAllLiveQueries) abstraction.

## <span id="parse"> Parsing SQL Statements

```java
List<ParsedStatement> parse(
  String sql)
```

`parse` requests the [EngineContext](#primaryContext) to [parse the given SQL statements](EngineContext.md#parse) (into a collection of `ParsedStatement`s).

`parse` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#parse) abstraction.

## <span id="prepare"> Preparing Statement for Execution

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
```

`prepare` requests the [EngineContext](#primaryContext) to [prepare the given ParsedStatement](EngineContext.md#prepare).

`prepare` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#prepare) abstraction.

## <span id="executeScalablePushQuery"> Executing Scalable Push Query

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

`executeScalablePushQuery` [creates an EngineExecutor](EngineExecutor.md#create) to [execute a scalable push query](EngineExecutor.md#executeScalablePushQuery).

`executeScalablePushQuery` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#executeScalablePushQuery) abstraction.

## <span id="executeTablePullQuery"> Executing Table Pull Query

```java
PullQueryResult executeTablePullQuery(
  ImmutableAnalysis analysis,
  ServiceContext serviceContext,
  ConfiguredStatement<Query> statement,
  HARouting routing,
  RoutingOptions routingOptions,
  QueryPlannerOptions plannerOptions,
  Optional<PullQueryExecutorMetrics> pullQueryMetrics,
  boolean startImmediately,
  Optional<ConsistencyOffsetVector> consistencyOffsetVector)
```

`executeTablePullQuery`...FIXME

`executeTablePullQuery` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#executeTablePullQuery) abstraction.

## <span id="isExecutableStatement"> isExecutableStatement

```java
boolean isExecutableStatement(
  Statement statement)
```

`isExecutableStatement` is positive (`true`) when the given [Statement](parser/Statement.md) is one of the following:

* [ExecutableDdlStatement](parser/ExecutableDdlStatement.md)
* [QueryContainer](parser/QueryContainer.md)
* [Query](parser/Query.md)

`isExecutableStatement` is used when:

* `EngineExecutor` is requested to [throwOnNonExecutableStatement](EngineExecutor.md#throwOnNonExecutableStatement)
* `RequestValidator` is requested to [validate](rest/RequestValidator.md#validate)

## <span id="analyzeQueryWithNoOutputTopic"> analyzeQueryWithNoOutputTopic

```java
ImmutableAnalysis analyzeQueryWithNoOutputTopic(
  Query query,
  String queryText,
  Map<String, Object> configOverrides)
```

`analyzeQueryWithNoOutputTopic`...FIXME

`analyzeQueryWithNoOutputTopic` is used when:

* `QueryExecutor` is requested to [handle pull or push queries](rest/QueryExecutor.md#handleQuery)

## <span id="executeTransientQuery"> Executing Transient Query

```java
TransientQueryMetadata executeTransientQuery(
  ServiceContext serviceContext,
  ConfiguredStatement<Query> statement,
  boolean excludeTombstones)
```

`executeTransientQuery` [creates an EngineExecutor](EngineExecutor.md#create) to [executeTransientQuery](EngineExecutor.md#executeTransientQuery).

`executeTransientQuery` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#executeTransientQuery) abstraction.

## <span id="createStreamPullQuery"> Creating Stream Pull Query

```java
StreamPullQueryMetadata createStreamPullQuery(
  ServiceContext serviceContext,
  ImmutableAnalysis analysis,
  ConfiguredStatement<Query> statementOrig,
  boolean excludeTombstones)
```

`createStreamPullQuery`...FIXME

`createStreamPullQuery` is used when:

* `QueryExecutor` is requested to [handle a stream pull query](rest/QueryExecutor.md#handleStreamPullQuery)
