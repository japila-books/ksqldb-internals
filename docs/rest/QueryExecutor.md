# QueryExecutor

## Creating Instance

`QueryExecutor` takes the following to be created:

* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* <span id="ksqlRestConfig"> [KsqlRestConfig](KsqlRestConfig.md)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="scalablePushQueryMetrics"> `ScalablePushQueryMetrics`
* <span id="rateLimiter"> `RateLimiter`
* <span id="concurrencyLimiter"> `ConcurrencyLimiter`
* <span id="pullBandRateLimiter"> [Pull Band Rate Limiter](../SlidingWindowRateLimiter.md)
* <span id="scalablePushBandRateLimiter"> [Scalable Push Band Rate Limiter](../SlidingWindowRateLimiter.md)
* [HARouting](#routing)
* <span id="pushRouting"> [PushRouting](../PushRouting.md)
* <span id="localCommand"> `LocalCommands`

`QueryExecutor` is created when:

* `KsqlRestApplication` is used to [build a KsqlRestApplication](KsqlRestApplication.md#buildApplication)

### <span id="routing"> HARouting

`QueryExecutor` is given a [HARouting](../HARouting.md) when [created](#creating-instance).

The `HARouting` is used to [handleTablePullQuery](#handleTablePullQuery) (for the [KsqlExecutionContext](#ksqlEngine) to [executeTablePullQuery](../KsqlExecutionContext.md#executeTablePullQuery)).

## <span id="handleStatement"> Handling Statement

```java
QueryMetadataHolder handleStatement(
  ServiceContext serviceContext,
  Map<String, Object> configOverrides,
  Map<String, Object> requestProperties,
  PreparedStatement<?> statement,
  Optional<Boolean> isInternalRequest,
  MetricsCallbackHolder metricsCallbackHolder,
  Context context,
  boolean excludeTombstones)
```

For the given `PreparedStatement` for a [Query](../parser/Query.md), `handleStatement` [handles it](#handleQuery). Otherwise, `handleStatement` returns an empty `QueryMetadataHolder`.

---

`handleStatement` is used when:

* `QueryEndpoint` is requested to [createQueryPublisher](QueryEndpoint.md#createQueryPublisher)
* `StreamedQueryResource` is requested to [handleStatement](StreamedQueryResource.md#handleStatement)
* `WSQueryEndpoint` is requested to [handleStatement](WSQueryEndpoint.md#handleStatement)

### <span id="handleQuery"> Handling Query Statement

```java
QueryMetadataHolder handleQuery(
  ServiceContext serviceContext,
  PreparedStatement<Query> statement,
  Optional<Boolean> isInternalRequest,
  MetricsCallbackHolder metricsCallbackHolder,
  Map<String, Object> configOverrides,
  Map<String, Object> requestProperties,
  Context context,
  boolean excludeTombstones)
```

#### <span id="handleQuery-pull-query"><span id="handleQuery-pull-query-stream"><span id="handleQuery-pull-query-table"> Pull Query

For a [pull query](../parser/Query.md#isPullQuery), `handleQuery` requests the [KsqlEngine](#ksqlEngine) to [analyze the query statement with no sink](../KsqlEngine.md#analyzeQueryWithNoOutputTopic) (that gives an [ImmutableAnalysis](../analyzer/ImmutableAnalysis.md)).

With [ksql.pull.queries.enable](../KsqlConfig.md#KSQL_PULL_QUERIES_ENABLE_CONFIG) disabled, `handleQuery` throws a `KsqlStatementException`:

```text
Pull queries are disabled.
```

`handleQuery` determines a `ConsistencyOffsetVector` based on [ksql.query.pull.consistency.token.enabled](../KsqlConfig.md#KSQL_QUERY_PULL_CONSISTENCY_OFFSET_VECTOR_ENABLED) in the [KsqlConfig](#ksqlConfig) and the given `requestProperties`.

`handleQuery` determines the [DataSourceType](../DataSource.md#DataSourceType) of the [FROM](../analyzer/ImmutableAnalysis.md#getFrom) clause (of the [DataSource](../DataSource.md) of the [ImmutableAnalysis](../analyzer/ImmutableAnalysis.md) of this pull query).

For a `KTABLE` data source, `handleQuery` [handle the table pull query](#handleTablePullQuery).

For a `KSTREAM` data source, `handleQuery` [handle the stream pull query](#handleStreamPullQuery).

#### <span id="handleQuery-scalable-push-query"> Scalable Push Query

For a [scalable push query](ScalablePushUtil.md#isScalablePushQuery), `handleQuery` requests the [KsqlEngine](#ksqlEngine) to [analyze the query statement with no sink](../KsqlEngine.md#analyzeQueryWithNoOutputTopic) (that gives an [ImmutableAnalysis](../analyzer/ImmutableAnalysis.md)).

`handleQuery` prints out the following INFO message to the logs:

```text
Scalable push query created
```

`handleQuery` [handleScalablePushQuery](#handleScalablePushQuery).

#### <span id="handleQuery-transient-query"> Transient Query

Otherwise, `handleQuery` prints out the following INFO message to the logs and [handlePushQuery](#handlePushQuery).

```text
Transient query created
```

### <span id="handlePushQuery"> handlePushQuery

```java
QueryMetadataHolder handlePushQuery(
  ServiceContext serviceContext,
  PreparedStatement<Query> statement,
  Map<String, Object> streamsProperties,
  boolean excludeTombstones)
```

`handlePushQuery`...FIXME

### <span id="handleScalablePushQuery"> handleScalablePushQuery

```java
QueryMetadataHolder handleScalablePushQuery(
  ImmutableAnalysis analysis,
  ServiceContext serviceContext,
  PreparedStatement<Query> statement,
  Map<String, Object> configOverrides,
  Map<String, Object> requestProperties,
  Context context,
  SlidingWindowRateLimiter scalablePushBandRateLimiter,
  AtomicReference<ScalablePushQueryMetadata> resultForMetrics)
```

`handleScalablePushQuery`...FIXME

`handleScalablePushQuery` requests the [KsqlEngine](#ksqlEngine) to [execute a scalable push query](../KsqlEngine.md#executeScalablePushQuery).

`handleScalablePushQuery`...FIXME

In the end, `handleScalablePushQuery` prints out the following INFO message to the logs:

```text
Streaming scalable push query
```

---

`handleScalablePushQuery` is used when:

* `QueryExecutor` is requested to [handle a query](#handleQuery-scalable-push-query)

### <span id="handleStreamPullQuery"> Handling Stream Pull Query

```java
QueryMetadataHolder handleStreamPullQuery(
  ImmutableAnalysis analysis,
  ServiceContext serviceContext,
  ConfiguredStatement<Query> configured,
  AtomicReference<StreamPullQueryMetadata> resultForMetrics,
  AtomicReference<Decrementer> refDecrementer)
```

In summary, `handleStreamPullQuery` requests the [KsqlExecutionContext](#ksqlEngine) for a [stream pull query](../KsqlExecutionContext.md#createStreamPullQuery) (that gives a `StreamPullQueryMetadata` to be returned inside a `QueryMetadataHolder`).

---

`handleStreamPullQuery` requests the [RateLimiter](#rateLimiter) to `checkLimit`.

`handleStreamPullQuery` requests the [pullBandRateLimiter](#pullBandRateLimiter) to [allow](../SlidingWindowRateLimiter.md#allow) a [PULL](../KsqlQueryType.md#PULL) query.

`handleStreamPullQuery` requests the [KsqlExecutionContext](#ksqlEngine) for a [stream pull query](../KsqlExecutionContext.md#createStreamPullQuery) (that gives a `StreamPullQueryMetadata`).

`handleStreamPullQuery` requests the [LocalCommands](#localCommands) (if defined) to `write` the `TransientQueryMetadata`.

In the end, `handleStreamPullQuery` gives a `QueryMetadataHolder` with the `StreamPullQueryMetadata`.

---

`handleStreamPullQuery` is used to [handle a stream pull query](#handleQuery-pull-query-stream).

### <span id="handleTablePullQuery"> handleTablePullQuery

```java
QueryMetadataHolder handleTablePullQuery(
  ImmutableAnalysis analysis,
  ServiceContext serviceContext,
  ConfiguredStatement<Query> configured,
  Map<String, Object> requestProperties,
  Optional<Boolean> isInternalRequest,
  SlidingWindowRateLimiter pullBandRateLimiter,
  AtomicReference<PullQueryResult> resultForMetrics,
  Optional<ConsistencyOffsetVector> consistencyOffsetVector)
```

`handleTablePullQuery` creates a `PullQueryConfigRoutingOptions` and a `PullQueryConfigPlannerOptions`.

In the end, `handleTablePullQuery` requests the [KsqlEngine](#ksqlEngine) to [executeTablePullQuery](../KsqlEngine.md#executeTablePullQuery).
