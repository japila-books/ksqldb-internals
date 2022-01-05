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

Part of _old API endpoints_

Used when:

* `ServerVerticle` is requested to [handle a KsqlRequest](ServerVerticle.md#handleKsqlRequest)

## Implementations

* [KsqlServerEndpoints](../rest/KsqlServerEndpoints.md)
