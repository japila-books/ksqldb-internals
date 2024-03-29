# KsqlEngine

`KsqlEngine` is the ksqlDB execution engine in all the available execution modes:

* [Embedded](embedded/index.md) (for [KsqlContext](embedded/KsqlContext.md#ksqlEngine))
* [Headless](headless/index.md) (for [StandaloneExecutor](headless/StandaloneExecutor.md#ksqlEngine))
* [REST](rest/index.md) (for [KsqlRestApplication](rest/KsqlRestApplication.md#ksqlEngine))

`KsqlEngine` is a facade (_frontend_) of the [EngineContext](#primaryContext).

`KsqlEngine` is used to [parse](#parse), validate (_executed_ in a [sandbox](#createSandbox)) and execute ksql statements (with starting [persistent queries](#getPersistentQueries)).

Phase | Embedded | Headless | REST
------|----------|----------|------
[Parsing](#parse) | [KsqlContext](embedded/KsqlContext.md#sql) | [StandaloneExecutor](headless/StandaloneExecutor.md#processesQueryFile) | [StatementParser](rest/StatementParser.md#parseSingleStatement)
Validation | [KsqlContext](embedded/KsqlContext.md#execute) | [StandaloneExecutor](headless/StandaloneExecutor.md#validateStatements) |
Execution | [KsqlContext](embedded/KsqlContext.md#execute) | [StandaloneExecutor](headless/StandaloneExecutor.md#executeStatements) | [InteractiveStatementExecutor](rest/InteractiveStatementExecutor.md#handleStatementWithTerminatedQueries)

## Creating Instance

`KsqlEngine` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* [ProcessingLogContext](#processingLogContext)
* [Service ID](#serviceId)
* [MutableMetaStore](#metaStore)
* <span id="engineMetricsFactory"> Function to [create a KsqlEngineMetrics](#engineMetrics)
* <span id="queryIdGenerator"> `QueryIdGenerator`
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="queryEventListeners"> `QueryEventListener`s

`KsqlEngine` is created when:

* `KsqlContext` is requested to [create](embedded/KsqlContext.md#create)
* `KsqlRestApplication` is requested to [buildApplication](rest/KsqlRestApplication.md#buildApplication)
* `StandaloneExecutorFactory` is requested to [create](headless/StandaloneExecutorFactory.md#create)

### <span id="processingLogContext"> ProcessingLogContext

`KsqlEngine` is given a [ProcessingLogContext](processing-log/ProcessingLogContext.md) when [created](#creating-instance).

The `ProcessingLogContext` is used to [create a primary context](#primaryContext) ([EngineContext](EngineContext.md#processingLogContext)).

### <span id="serviceId"><span id="getServiceId"> Service ID

`KsqlEngine` can be given a **Service ID** when [created](#creating-instance). Unless defined, the Service ID is from [ServiceInfo](ServiceInfo.md#serviceId) based on [ksql.service.id](KsqlConfig.md#KSQL_SERVICE_ID_CONFIG) configuration property.

The service ID is used (via `getServiceId` getter) to create the following:

* [KsqlEngineMetrics](KsqlEngineMetrics.md#newCustomMetricsTags)
* [KsqlRestApplication](rest/KsqlRestApplication.md#buildApplication)

### <span id="metaStore"> MutableMetaStore

`KsqlEngine` can be given a [MutableMetaStore](MutableMetaStore.md) when [created](#creating-instance).

The `MutableMetaStore` is used to create the [EngineContext](#primaryContext).

### <span id="primaryContext"> EngineContext

`KsqlEngine` [creates an EngineContext](EngineContext.md#create) when [created](#creating-instance).

`KsqlEngine` delegates most of its processing to the `EngineContext` (directly or indirectly using[EngineExecutor](EngineExecutor.md#create)) and is (pretty much) their facade.

### <span id="engineMetrics"> KsqlEngineMetrics

`KsqlEngine` creates a [KsqlEngineMetrics](KsqlEngineMetrics.md) (using the [engineMetricsFactory](#engineMetricsFactory)) when [created](#creating-instance).

The `KsqlEngineMetrics` is used for the following:

* [Accessing the QueryStateMetricsReportingListener](KsqlEngineMetrics.md#getQueryEventListener) when this `KsqlEngine` is [created](#creating-instance)
* [Updating runtime metrics](#updateMetrics)

## <span id="aggregateMetricsCollector"><span id="updateMetrics"> Runtime Metrics

`KsqlEngine` creates a single-threaded Java `Executor` for [updating performance metrics](KsqlEngineMetrics.md#updateMetrics) (using [KsqlEngineMetrics](#engineMetrics)) every 1 second.

`KsqlEngine` stops updating metrics when [closed](#close).

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

`execute` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#execute) abstraction.

---

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

`executeScalablePushQuery` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#executeScalablePushQuery) abstraction.

---

`executeScalablePushQuery` [creates an EngineExecutor](EngineExecutor.md#create) to [execute a scalable push query](EngineExecutor.md#executeScalablePushQuery).

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

`executeTablePullQuery` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#executeTablePullQuery) abstraction.

---

`executeTablePullQuery`...FIXME

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

## <span id="analyzeQueryWithNoOutputTopic"> Analyzing Query Statement With No Sink

```java
ImmutableAnalysis analyzeQueryWithNoOutputTopic(
  Query query,
  String queryText,
  Map<String, Object> configOverrides)
```

`analyzeQueryWithNoOutputTopic` creates a [QueryAnalyzer](analyzer/QueryAnalyzer.md) with the following:

* [ksql.rowpartition.rowoffset.enabled](#getRowpartitionRowoffsetEnabled)
* [ksql.query.pull.limit.clause.enabled](KsqlConfig.md#KSQL_QUERY_PULL_LIMIT_CLAUSE_ENABLED)

`analyzeQueryWithNoOutputTopic` requests the `QueryAnalyzer` to [analyze](analyzer/QueryAnalyzer.md#analyze) the given [Query](parser/Query.md) statement (and no [Sink](parser/Sink.md)).

In the end, `analyzeQueryWithNoOutputTopic` creates a [RewrittenAnalysis](analyzer/RewrittenAnalysis.md) (with the [Analysis](analyzer/Analysis.md) and `QueryExecutionUtil.ColumnReferenceRewriter`)

---

`analyzeQueryWithNoOutputTopic` is used when:

* `QueryExecutor` is requested to [handle pull or push queries](rest/QueryExecutor.md#handleQuery)

### <span id="getRowpartitionRowoffsetEnabled"> getRowpartitionRowoffsetEnabled

```java
boolean getRowpartitionRowoffsetEnabled(
  KsqlConfig ksqlConfig,
  Map<String, Object> configOverrides)
```

`getRowpartitionRowoffsetEnabled` takes the value of [ksql.rowpartition.rowoffset.enabled](KsqlConfig.md#KSQL_ROWPARTITION_ROWOFFSET_ENABLED) from `configOverrides` (if available) or the given [KsqlConfig](KsqlConfig.md).

## <span id="executeTransientQuery"> Executing Transient Query

```java
TransientQueryMetadata executeTransientQuery(
  ServiceContext serviceContext,
  ConfiguredStatement<Query> statement,
  boolean excludeTombstones)
```

`executeTransientQuery` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#executeTransientQuery) abstraction.

---

`executeTransientQuery` [creates an EngineExecutor](EngineExecutor.md#create) to [executeTransientQuery](EngineExecutor.md#executeTransientQuery).

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

## <span id="updateStreamsPropertiesAndRestartRuntime"> updateStreamsPropertiesAndRestartRuntime

```java
void updateStreamsPropertiesAndRestartRuntime()
```

`updateStreamsPropertiesAndRestartRuntime` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#updateStreamsPropertiesAndRestartRuntime) abstraction.

---

`updateStreamsPropertiesAndRestartRuntime`...FIXME

## <span id="getMetaStore"> getMetaStore

```java
MetaStore getMetaStore()
```

`getMetaStore` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#getMetaStore) abstraction.

---

`getMetaStore` requests the [EngineContext](#primaryContext) for the [MetaStore](EngineContext.md#getMetaStore)

## <span id="getPersistentQueries"> getPersistentQueries

```java
List<PersistentQueryMetadata> getPersistentQueries()
```

`getPersistentQueries` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#getPersistentQueries) abstraction.

---

`getPersistentQueries` requests the [EngineContext](#primaryContext) for the [QueryRegistry](EngineContext.md#getQueryRegistry) for the [persistent queries](QueryRegistry.md#getPersistentQueries).

## Demo: Creating KsqlEngine

```scala
import io.confluent.ksql.util.KsqlConfig
val ksqlConfig = KsqlConfig.empty

import io.confluent.ksql.services._
val serviceContext = ServiceContextFactory.create(ksqlConfig, () => DisabledKsqlClient.instance)

import io.confluent.ksql.logging.processing.ProcessingLogContext
val processingLogContext = ProcessingLogContext.create()

import io.confluent.ksql.function.InternalFunctionRegistry
val functionRegistry = new InternalFunctionRegistry()

import io.confluent.ksql.ServiceInfo
val serviceInfo = ServiceInfo.create(ksqlConfig)

import io.confluent.ksql.query.id.SequentialQueryIdGenerator
val queryIdGenerator = new SequentialQueryIdGenerator()

import io.confluent.ksql.engine.QueryEventListener
import scala.jdk.CollectionConverters._
val queryEventListeners = Seq.empty[QueryEventListener].asJava

import io.confluent.ksql.metrics.MetricCollectors
val metricCollectors = new MetricCollectors()
```

```scala
import io.confluent.ksql.engine.KsqlEngine
val ksqlEngine = new KsqlEngine(
  serviceContext,
  processingLogContext,
  functionRegistry,
  serviceInfo,
  queryIdGenerator,
  ksqlConfig,
  queryEventListeners,
  metricCollectors)
```

## Logging

Enable `ALL` logging level for `io.confluent.ksql.engine.KsqlEngine` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.io.confluent.ksql.engine.KsqlEngine=ALL
```

Refer to [Logging](logging.md).
