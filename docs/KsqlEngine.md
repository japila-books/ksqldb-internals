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

`execute` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#execute) abstraction.

---

1. [Plans the statement](#plan) and creates a `ConfiguredKsqlPlan` for the other `execute`

`execute` [creates an EngineExecutor](EngineExecutor.md#create) to [execute](EngineExecutor.md#execute) the [KsqlPlan](KsqlPlan.md) (of the given `ConfiguredKsqlPlan`).

## <span id="plan"> Statement Planning (plan)

```java
KsqlPlan plan(
  ServiceContext serviceContext,
  ConfiguredStatement<?> statement)
```

`plan` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#plan) abstraction.

---

`plan` [creates an EngineExecutor](EngineExecutor.md#create) to [plan](EngineExecutor.md#plan) the given `ConfiguredStatement`.

## <span id="getAllLiveQueries"> getAllLiveQueries

```java
List<QueryMetadata> getAllLiveQueries()
```

`getAllLiveQueries` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#getAllLiveQueries) abstraction.

---

`getAllLiveQueries` requests the [EngineContext](#primaryContext) for the [QueryRegistry](EngineContext.md#getQueryRegistry) that is then requested for [all live queries](QueryRegistry.md#getAllLiveQueries).

## <span id="parse"> Parsing SQL Statements

```java
List<ParsedStatement> parse(
  String sql)
```

`parse` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#parse) abstraction.

---

`parse` requests the [EngineContext](#primaryContext) to [parse the given SQL statements](EngineContext.md#parse) (into a collection of `ParsedStatement`s).

## <span id="prepare"> Preparing Statement for Execution

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
```

`prepare` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#prepare) abstraction.

---

`prepare` requests the [EngineContext](#primaryContext) to [prepare the given ParsedStatement](EngineContext.md#prepare).

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

---

`isExecutableStatement` is used when:

* `EngineExecutor` is requested to [plan a statement](EngineExecutor.md#plan) (and [throwOnNonExecutableStatement](EngineExecutor.md#throwOnNonExecutableStatement))
* `RequestValidator` is requested to [validate a statement](rest/RequestValidator.md#validate)

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

`createStreamPullQuery` uses [ksql.query.pull.stream.enabled](KsqlConfig.md#KSQL_QUERY_STREAM_PULL_QUERY_ENABLED) to ensure that pull queries on streams are enabled. If not, `createStreamPullQuery` throws a `KsqlStatementException`:

```text
Pull queries on streams are disabled.
To create a push query on the stream, add EMIT CHANGES to the end.
To enable pull queries on streams, set the ksql.query.pull.stream.enabled config to 'true'.
```

`createStreamPullQuery`...FIXME

`createStreamPullQuery` [creates an EngineExecutor](EngineExecutor.md#create) to [execute the stream pull query](EngineExecutor.md#executeStreamPullQuery).

`createStreamPullQuery`...FIXME

In the end, `createStreamPullQuery` returns a `StreamPullQueryMetadata` with the `TransientQueryMetadata` and the `endOffsets`.

---

`createStreamPullQuery` is used when:

* `QueryExecutor` is requested to [handle a stream pull query](rest/QueryExecutor.md#handleStreamPullQuery)
