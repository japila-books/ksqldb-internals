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
  ConfiguredStatement<?> statement) // (1)
ExecuteResult execute(
  ServiceContext serviceContext,
  ConfiguredKsqlPlan plan)
```

1. [Plans the statement](#plan) and creates a `ConfiguredKsqlPlan`

`execute` [creates an EngineExecutor](EngineExecutor.md#create) to [execute](EngineExecutor.md#execute) the `KsqlPlan` (of the `ConfiguredKsqlPlan`).

`execute` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#execute) abstraction.

## <span id="plan"> Execution Planning

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

## <span id="parse"> Parsing SQL Text

```java
List<ParsedStatement> parse(
  String sql)
```

`parse`...FIXME

`parse` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#parse) abstraction.

## <span id="prepare"> Preparing ParsedStatement

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
```

`prepare` requests the [EngineContext](#primaryContext) to [prepare the given ParsedStatement](EngineContext.md#prepare).

`prepare` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#prepare) abstraction.

## <span id="executeTablePullQuery"> executeTablePullQuery

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
