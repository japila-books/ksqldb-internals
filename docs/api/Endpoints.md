# Endpoints

`Endpoints` is an [abstraction](#contract) of [API endpoints](#implementations).

## Contract (Subset)

### <span id="createQueryPublisher"> Creating QueryPublisher

```java
CompletableFuture<QueryPublisher> createQueryPublisher(
  String sql,
  Map<String, Object> properties,
  Map<String, Object> sessionVariables,
  Map<String, Object> requestProperties,
  Context context, WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext,
  MetricsCallbackHolder metricsCallbackHolder,
  Optional<Boolean> isInternalRequest)
```

See [KsqlServerEndpoints](../rest/KsqlServerEndpoints.md#createQueryPublisher)

Used when:

* `ServerVerticle` is requested to [handle /query-stream and /query REST endpoints](ServerVerticle.md#setupRouter)

### <span id="createInsertsSubscriber"> Creating InsertsStreamSubscriber

```java
CompletableFuture<InsertsStreamSubscriber> createInsertsSubscriber(
  String target,
  JsonObject properties,
  Subscriber<InsertResult> acksSubscriber,
  Context context,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext)
```

See [KsqlServerEndpoints](../rest/KsqlServerEndpoints.md#createInsertsSubscriber)

Used when:

* `ServerVerticle` is requested to [handle /inserts-stream REST endpoint](ServerVerticle.md#setupRouter)

### <span id="executeKsqlRequest"> Executing KsqlRequest

```java
CompletableFuture<EndpointResponse> executeKsqlRequest(
  KsqlRequest request,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext)
```

See [KsqlServerEndpoints](../rest/KsqlServerEndpoints.md#executeKsqlRequest)

Used when:

* `ServerVerticle` is requested to [handle a KsqlRequest](ServerVerticle.md#handleKsqlRequest)

### <span id="executeQueryRequest"> Executing Query Request

```java
CompletableFuture<EndpointResponse> executeQueryRequest(
  KsqlRequest request, WorkerExecutor workerExecutor,
  CompletableFuture<Void> connectionClosedFuture, ApiSecurityContext apiSecurityContext,
  Optional<Boolean> isInternalRequest,
  KsqlMediaType mediaType,
  MetricsCallbackHolder metricsCallbackHolder,
  Context context)
```

See [KsqlServerEndpoints](../rest/KsqlServerEndpoints.md#executeQueryRequest)

Used when:

* `ServerVerticle` is requested to [handle a query request](ServerVerticle.md#handleQueryRequest)

### <span id="executeWebsocketStream"> executeWebsocketStream

```java
void executeWebsocketStream(
  ServerWebSocket webSocket,
  MultiMap requstParams,
  WorkerExecutor workerExecutor,
  ApiSecurityContext apiSecurityContext,
  Context context)
```

See [KsqlServerEndpoints](../rest/KsqlServerEndpoints.md#executeWebsocketStream)

Used when:

* `ServerVerticle` is requested to [handleWebsocket](ServerVerticle.md#handleWebsocket) (for `GET /ws/query` HTTP requests)

## Implementations

* [KsqlServerEndpoints](../rest/KsqlServerEndpoints.md)
