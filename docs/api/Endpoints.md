# Endpoints

`Endpoints` is an [abstraction](#contract) of [API endpoints](#implementations).

## Contract (Subset)

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
