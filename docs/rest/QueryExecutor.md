# QueryExecutor

## Creating Instance

`QueryExecutor` takes the following to be created:

* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* <span id="ksqlRestConfig"> `KsqlRestConfig`
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="scalablePushQueryMetrics"> `ScalablePushQueryMetrics`
* <span id="rateLimiter"> `RateLimiter`
* <span id="concurrencyLimiter"> `ConcurrencyLimiter`
* <span id="pullBandRateLimiter"> `SlidingWindowRateLimiter`
* <span id="scalablePushBandRateLimiter"> `SlidingWindowRateLimiter`
* <span id="routing"> `HARouting`
* <span id="pushRouting"> `PushRouting`
* <span id="localCommand"> `LocalCommands`

`QueryExecutor` is created when:

* `KsqlRestApplication` utility is used to [build a KsqlRestApplication](KsqlRestApplication.md#buildApplication)

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

For a [Query](../parser/Query.md) statement, `handleStatement` [handles it](#handleQuery). Otherwise, `handleStatement` returns an empty `QueryMetadataHolder`.

`handleStatement` is used when:

* `QueryEndpoint` is requested to [createQueryPublisher](QueryEndpoint.md#createQueryPublisher)
* `StreamedQueryResource` is requested to `handleStatement`
* `WSQueryEndpoint` is requested to `handleStatement`

## <span id="handleQuery"> Handling Query

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

`handleQuery` is used when:

* `QueryExecutor` is requested to [handle a statement](#handleStatement)

### Pull Queries

For a [pull query](../parser/Query.md#isPullQuery), `handleQuery` requests the [KsqlEngine](#ksqlEngine) to [analyzeQueryWithNoOutputTopic](../KsqlEngine.md#analyzeQueryWithNoOutputTopic) (that gives an [ImmutableAnalysis](../ImmutableAnalysis.md)).

With [ksql.pull.queries.enable](../KsqlConfig.md#KSQL_PULL_QUERIES_ENABLE_CONFIG) disabled, `handleQuery` throws a `KsqlStatementException`:

```text
Pull queries are disabled.
```

`handleQuery` determines a `ConsistencyOffsetVector` based on [ksql.query.pull.consistency.token.enabled](../KsqlConfig.md#KSQL_QUERY_PULL_CONSISTENCY_OFFSET_VECTOR_ENABLED) in the [KsqlConfig](#ksqlConfig) and the given `requestProperties`.

For a `KTABLE` data source (`FROM` clause), `handleQuery` [handleTablePullQuery](#handleTablePullQuery).

For a `KSTREAM` data source (`FROM` clause), `handleQuery` [handleStreamPullQuery](#handleStreamPullQuery).

### <span id="handleQuery-scalable-push-query"> Scalable Push Queries

For a [scalable push query](ScalablePushUtil.md#isScalablePushQuery), `handleQuery` requests the [KsqlEngine](#ksqlEngine) to [analyzeQueryWithNoOutputTopic](../KsqlEngine.md#analyzeQueryWithNoOutputTopic) (that gives an [ImmutableAnalysis](../ImmutableAnalysis.md)).

`handleQuery` prints out the following INFO message to the logs:

```text
Scalable push query created
```

`handleQuery` [handleScalablePushQuery](#handleScalablePushQuery).

### Transient Queries

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

### <span id="handleScalablePushQuery"> Executing Scalable Push Query (handleScalablePushQuery)

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

`handleScalablePushQuery` requests the [KsqlEngine](#ksqlEngine) to [execute a scalable push query](../KsqlEngine.md#executeScalablePushQuery).

In the end, `handleScalablePushQuery` prints out the following INFO message to the logs:

```text
Streaming scalable push query
```

`handleScalablePushQuery` is used when:

* `QueryExecutor` is requested to [handle a Query](#handleQuery-scalable-push-query)

### <span id="handleStreamPullQuery"> handleStreamPullQuery

```java
QueryMetadataHolder handleStreamPullQuery(
  ImmutableAnalysis analysis,
  ServiceContext serviceContext,
  ConfiguredStatement<Query> configured,
  AtomicReference<StreamPullQueryMetadata> resultForMetrics,
  AtomicReference<Decrementer> refDecrementer)
```

`handleStreamPullQuery`...FIXME

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

`handleTablePullQuery`...FIXME
