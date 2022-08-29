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

## Implementations

* [KsqlServerEndpoints](../rest/KsqlServerEndpoints.md)
