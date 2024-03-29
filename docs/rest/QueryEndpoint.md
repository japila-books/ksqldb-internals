# QueryEndpoint

`QueryEndpoint` is used to [create a QueryPublisher](#createQueryPublisher) for [KsqlServerEndpoints](KsqlServerEndpoints.md#createQueryPublisher).

## Creating Instance

`QueryEndpoint` takes the following to be created:

* [KsqlEngine](#ksqlEngine)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* [QueryExecutor](#queryExecutor)

`QueryEndpoint` is created when:

* `KsqlServerEndpoints` is requested to [createQueryPublisher](KsqlServerEndpoints.md#createQueryPublisher)

### <span id="ksqlEngine"> KsqlEngine

`QueryEndpoint` is given a [KsqlEngine](../KsqlEngine.md) when [created](#creating-instance).

The `KsqlEngine` is used when [configuring a query statement](#createStatement) (to [parse](../KsqlExecutionContext.md#parse) and [prepare](../KsqlExecutionContext.md#prepare) a KSQL query statement).

### <span id="queryExecutor"> QueryExecutor

`QueryEndpoint` is given a [QueryExecutor](QueryExecutor.md) when [created](#creating-instance).

The `QueryExecutor` is used for [createQueryPublisher](#createQueryPublisher) (to [handle a configured KSQL query statement](QueryExecutor.md#handleStatement)).

## <span id="createQueryPublisher"> Creating QueryPublisher

```java
QueryPublisher createQueryPublisher(
  String sql,
  Map<String, Object> properties,
  Map<String, Object> sessionVariables,
  Map<String, Object> requestProperties,
  Context context,
  WorkerExecutor workerExecutor,
  ServiceContext serviceContext,
  MetricsCallbackHolder metricsCallbackHolder,
  Optional<Boolean> isInternalRequest)
```

`createQueryPublisher` [creates a ConfiguredStatement](#createStatement) for the given `sql` (that is expected to be a [Query](../parser/Query.md) statement).

`createQueryPublisher` requests the [QueryExecutor](#queryExecutor) to [handle the query statement](QueryExecutor.md#handleStatement).

For a pull query result, `createQueryPublisher` creates a [BlockingQueryPublisher](../api/BlockingQueryPublisher.md) to [setQueryHandle](../api/BlockingQueryPublisher.md#setQueryHandle) with a new `KsqlPullQueryHandle` and the flags:

* `isPullQuery` enabled
* `isScalablePushQuery` disabled

For a push query result, `createQueryPublisher` creates a [BlockingQueryPublisher](../api/BlockingQueryPublisher.md) to [setQueryHandle](../api/BlockingQueryPublisher.md#setQueryHandle) with a new `KsqlQueryHandle` and the flags:

* `isPullQuery` disabled
* `isScalablePushQuery` based on whether `QueryMetadataHolder` holds a `ScalablePushQueryMetadata` or not

Otherwise, `createQueryPublisher` throws an `KsqlStatementException`:

```text
Unexpected metadata for query
```

---

`createQueryPublisher` is used when:

* `KsqlServerEndpoints` is requested to [createQueryPublisher](KsqlServerEndpoints.md#createQueryPublisher)
