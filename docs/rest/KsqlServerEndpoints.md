# KsqlServerEndpoints

`KsqlServerEndpoints` is an [Endpoints](../api/Endpoints.md).

## Creating Instance

`KsqlServerEndpoints` takes the following to be created:

* [KsqlEngine](#ksqlEngine)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="ksqlSecurityContextProvider"> `KsqlSecurityContextProvider`
* <span id="ksqlResource"> [KsqlResource](KsqlResource.md)
* <span id="streamedQueryResource"> `StreamedQueryResource`
* <span id="serverInfoResource"> `ServerInfoResource`
* <span id="heartbeatResource"> `HeartbeatResource`
* <span id="clusterStatusResource"> `ClusterStatusResource`
* <span id="statusResource"> `StatusResource`
* <span id="lagReportingResource"> `LagReportingResource`
* <span id="healthCheckResource"> `HealthCheckResource`
* <span id="serverMetadataResource"> `ServerMetadataResource`
* <span id="wsQueryEndpoint"> `WSQueryEndpoint`
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="queryExecutor"> [QueryExecutor](QueryExecutor.md)
* <span id="authTokenProvider"> `KsqlAuthTokenProvider`

`KsqlServerEndpoints` is created when:

* `KsqlRestApplication` is requested to [startAsync](KsqlRestApplication.md#startAsync)

### <span id="ksqlEngine"> KsqlEngine

`KsqlServerEndpoints` is given a [KsqlEngine](../KsqlEngine.md) when [created](#creating-instance).

The `KsqlEngine` is used when:

* [createQueryPublisher](#createQueryPublisher) (to create a [QueryEndpoint](QueryEndpoint.md))
* [createInsertsSubscriber](#createInsertsSubscriber) (to create a `InsertsStreamEndpoint`)

## <span id="createQueryPublisher"> Creating QueryPublisher

```java
CompletableFuture<QueryPublisher> createQueryPublisher(
  String sql,
  Map<String, Object> properties,
  Map<String, Object> sessionVariables,
  Map<String, Object> requestProperties,
  Context context,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext,
  MetricsCallbackHolder metricsCallbackHolder,
  Optional<Boolean> isInternalRequest)
```

`createQueryPublisher` is part of the [Endpoints](../api/Endpoints.md#createQueryPublisher) abstraction.

---

`createQueryPublisher` [executes the following](#executeOnWorker) on the given `WorkerExecutor`:

* Create a [QueryEndpoint](QueryEndpoint.md) to [createQueryPublisher](QueryEndpoint.md#createQueryPublisher)

## <span id="executeKsqlRequest"> Executing KsqlRequest

```java
CompletableFuture<EndpointResponse> executeKsqlRequest(
  KsqlRequest request,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext)
```

`executeKsqlRequest` is part of the [Endpoints](../api/Endpoints.md#executeKsqlRequest) abstraction.

---

`executeKsqlRequest`...FIXME
