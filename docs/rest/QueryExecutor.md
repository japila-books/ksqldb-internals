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

* `KsqlRestApplication` is requested to [buildApplication](KsqlRestApplication.md#buildApplication)

## <span id="handleStatement"> handleStatement

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

`handleStatement`...FIXME

`handleStatement` is used when:

* `QueryEndpoint` is requested to [createQueryPublisher](#createQueryPublisher)
* `StreamedQueryResource` is requested to `handleStatement`
* `WSQueryEndpoint` is requested to `handleStatement`

### <span id="handleQuery"> handleQuery

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

`handleQuery`...FIXME

### <span id="handlePushQuery"> handlePushQuery

```java
QueryMetadataHolder handlePushQuery(
  ServiceContext serviceContext,
  PreparedStatement<Query> statement,
  Map<String, Object> streamsProperties,
  boolean excludeTombstones)
```

`handlePushQuery`...FIXME
