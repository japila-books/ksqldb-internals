# KsqlServerEndpoints

## Creating Instance

`KsqlServerEndpoints` takes the following to be created:

* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
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

`KsqlServerEndpoints` is created when:

* `KsqlRestApplication` is requested to [startAsync](KsqlRestApplication.md#startAsync)

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

`createQueryPublisher`...FIXME

`createQueryPublisher` is part of the [Endpoints](../api/Endpoints.md#createQueryPublisher) abstraction.

## <span id="executeKsqlRequest"> Executing KsqlRequest

```java
CompletableFuture<EndpointResponse> executeKsqlRequest(
  KsqlRequest request,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext)
```

`executeKsqlRequest`...FIXME

`executeKsqlRequest` is part of the [Endpoints](../api/Endpoints.md#executeKsqlRequest) abstraction.
