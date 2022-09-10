# KsqlServerEndpoints

`KsqlServerEndpoints` is an [Endpoints](../api/Endpoints.md).

## Creating Instance

`KsqlServerEndpoints` takes the following to be created:

* [KsqlEngine](#ksqlEngine)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="ksqlSecurityContextProvider"> `KsqlSecurityContextProvider`
* [KsqlResource](#ksqlResource)
* [StreamedQueryResource](#streamedQueryResource)
* <span id="serverInfoResource"> `ServerInfoResource`
* <span id="heartbeatResource"> `HeartbeatResource`
* <span id="clusterStatusResource"> `ClusterStatusResource`
* <span id="statusResource"> `StatusResource`
* <span id="lagReportingResource"> `LagReportingResource`
* <span id="healthCheckResource"> `HealthCheckResource`
* <span id="serverMetadataResource"> `ServerMetadataResource`
* <span id="wsQueryEndpoint"> [WSQueryEndpoint](WSQueryEndpoint.md)
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="queryExecutor"> [QueryExecutor](QueryExecutor.md)
* <span id="authTokenProvider"> `KsqlAuthTokenProvider`

`KsqlServerEndpoints` is created when:

* `KsqlRestApplication` is requested to [startAsync](KsqlRestApplication.md#startAsync)

### <span id="ksqlEngine"> KsqlEngine

`KsqlServerEndpoints` is given a [KsqlEngine](../KsqlEngine.md) when [created](#creating-instance).

The `KsqlEngine` is used when:

* [createQueryPublisher](#createQueryPublisher) (to create a [QueryEndpoint](QueryEndpoint.md#ksqlEngine))
* [createInsertsSubscriber](#createInsertsSubscriber) (to create a [InsertsStreamEndpoint](InsertsStreamEndpoint.md#ksqlEngine))

### <span id="ksqlResource"> KsqlResource

`KsqlServerEndpoints` is given a [KsqlResource](KsqlResource.md) when [created](#creating-instance).

`KsqlResource` is used when:

* [Executing ksqlRequest](#executeKsqlRequest)
* [executeTerminate](#executeTerminate)
* [executeIsValidProperty](#executeIsValidProperty)

### <span id="streamedQueryResource"> StreamedQueryResource

`KsqlServerEndpoints` is given a [StreamedQueryResource](StreamedQueryResource.md) when [created](#creating-instance).

The `StreamedQueryResource` is used to [executeQueryRequest](#executeQueryRequest).

## <span id="createInsertsSubscriber"> Creating InsertsStreamSubscriber

```java
CompletableFuture<InsertsStreamSubscriber> createInsertsSubscriber(
  String target,
  JsonObject properties,
  Subscriber<InsertResult> acksSubscriber,
  Context context,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext)
```

`createInsertsSubscriber` is part of the [Endpoints](../api/Endpoints.md#createInsertsSubscriber) abstraction.

---

`createInsertsSubscriber` [executes the following](#executeOnWorker) on the given `WorkerExecutor`:

* Create an [InsertsStreamEndpoint](InsertsStreamEndpoint.md) to [createInsertsSubscriber](InsertsStreamEndpoint.md#createInsertsSubscriber)

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

## <span id="executeIsValidProperty"> executeIsValidProperty

```java
CompletableFuture<EndpointResponse> executeIsValidProperty(
  String property,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext)
```

`executeIsValidProperty` is part of the [Endpoints](../api/Endpoints.md#executeIsValidProperty) abstraction.

---

`executeIsValidProperty` requests the [KsqlResource](#ksqlResource) to [isValidProperty](KsqlResource.md#isValidProperty).

## <span id="executeKsqlRequest"> executeKsqlRequest

```java
CompletableFuture<EndpointResponse> executeKsqlRequest(
  KsqlRequest request,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext)
```

`executeKsqlRequest` is part of the [Endpoints](../api/Endpoints.md#executeKsqlRequest) abstraction.

---

`executeKsqlRequest` requests the [KsqlResource](#ksqlResource) to [handle statements](KsqlResource.md#handleKsqlStatements).

## <span id="executeQueryRequest"> executeQueryRequest

```java
CompletableFuture<EndpointResponse> executeQueryRequest(
  KsqlRequest request,
  WorkerExecutor workerExecutor,
  CompletableFuture<Void> connectionClosedFuture,
  ApiSecurityContext apiSecurityContext,
  Optional<Boolean> isInternalRequest,
  KsqlMediaType mediaType,
  MetricsCallbackHolder metricsCallbackHolder,
  Context context)
```

`executeQueryRequest` is part of the [Endpoints](../api/Endpoints.md#executeQueryRequest) abstraction.

---

`executeQueryRequest` requests the [StreamedQueryResource](#streamedQueryResource) to [streamQuery](KsqlResource.md#streamQuery).

## <span id="executeTerminate"> executeTerminate

```java
CompletableFuture<EndpointResponse> executeTerminate(
  ClusterTerminateRequest request,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext)
```

`executeTerminate` is part of the [Endpoints](../api/Endpoints.md#executeTerminate) abstraction.

---

`executeTerminate` requests the [KsqlResource](#ksqlResource) to [terminateCluster](KsqlResource.md#terminateCluster).

## <span id="executeWebsocketStream"> executeWebsocketStream

```java
void executeWebsocketStream(
  ServerWebSocket webSocket,
  MultiMap requestParams,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext,
  Context context)
```

`executeWebsocketStream` is part of the [Endpoints](../api/Endpoints.md#executeWebsocketStream) abstraction.

---

`executeWebsocketStream` requests the [WSQueryEndpoint](#wsQueryEndpoint) to [executeStreamQuery](WSQueryEndpoint.md#executeStreamQuery).
