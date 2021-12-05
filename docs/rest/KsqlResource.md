# KsqlResource

## <span id="handler"> RequestHandler

`KsqlResource` creates a `RequestHandler` when requested to [configure](#configure).

`RequestHandler` is used when:

* [terminateCluster](#terminateCluster)
* [handleKsqlStatements](#handleKsqlStatements)

## <span id="handleKsqlStatements"> handleKsqlStatements

```java
EndpointResponse handleKsqlStatements(
  KsqlSecurityContext securityContext,
  KsqlRequest request)
```

`handleKsqlStatements` prints out the following INFO message to the logs:

```text
Received: [request]
```

`handleKsqlStatements`...FIXME

`handleKsqlStatements` requests the [KsqlEngine](#ksqlEngine) to [parse the SQL text](../KsqlEngine.md#parse).

`handleKsqlStatements`...FIXME

`handleKsqlStatements` requests the [RequestHandler](#handler) to [execute the SQL statements](RequestHandler.md#execute).

`handleKsqlStatements`...FIXME

`handleKsqlStatements` is used when:

* `KsqlServerEndpoints` is requested to `executeKsqlRequest`
* `ServerInternalKsqlClient` is requested to `makeKsqlRequest`
