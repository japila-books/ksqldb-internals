# QueryEndpoint

`QueryEndpoint` is [created](#creating-instance) and requested to [createQueryPublisher](#createQueryPublisher) for [KsqlServerEndpoints](KsqlServerEndpoints.md) (when requested to [createQueryPublisher](KsqlServerEndpoints.md#createQueryPublisher)).

## Creating Instance

`QueryEndpoint` takes the following to be created:

* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="queryExecutor"> [QueryExecutor](QueryExecutor.md)

`QueryEndpoint` is created when:

* `KsqlServerEndpoints` is requested to [createQueryPublisher](KsqlServerEndpoints.md#createQueryPublisher)

## <span id="createQueryPublisher"> createQueryPublisher

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

`createQueryPublisher` [createStatement](#createStatement).

`createQueryPublisher` requests the [QueryExecutor](#queryExecutor) to [handleStatement](QueryExecutor.md#handleStatement).

`createQueryPublisher`...FIXME

---

`createQueryPublisher` is used when:

* `KsqlServerEndpoints` is requested to [createQueryPublisher](KsqlServerEndpoints.md#createQueryPublisher)
