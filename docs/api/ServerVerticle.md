# ServerVerticle

`ServerVerticle` is a `Verticle` ([Vert.x]({{ vertx.api }}/io/vertx/core/Verticle.html)).

> A verticle is a piece of code that can be deployed by Vert.x (...) to provide an _actor-like_ deployment and concurrency model, out of the box.

## Creating Instance

`ServerVerticle` takes the following to be created:

* <span id="endpoints"> [Endpoints](Endpoints.md)
* <span id="httpServerOptions"> `HttpServerOptions` ([Vert.x]({{ vertx.api }}/io/vertx/core/http/HttpServerOptions.html))
* <span id="server"> [Server](Server.md)
* <span id="isInternalListener"> `isInternalListener` flag
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="loggingRateLimiter"> `LoggingRateLimiter`

`ServerVerticle` is created when:

* `Server` is requested to [start](Server.md#start)

## <span id="start"> Starting Verticle

```java
void start(
  Promise<Void> startPromise)
```

`start` is part of the `Verticle` ([Vert.x]({{ vertx.api }}/io/vertx/core/Verticle.html#start-io.vertx.core.Promise-)) abstraction.

---

`start` creates a [ConnectionQueryManager](#connectionQueryManager).

`start` creates an `HttpServer` ([Vert.x]({{ vertx.api }}/io/vertx/core/Vertx.html#createHttpServer-io.vertx.core.http.HttpServerOptions-)) (with the [HttpServerOptions](#httpServerOptions)) and registers the [request handlers](#setupRouter).

## <span id="setupRouter"><span id="uris"> URIs

```java
Router setupRouter()
```

`setupRouter` registers the query handlers.

URI      | HTTP Method | Handler | Produces
---------|-------------|---------|---------
 `/` | `GET` | `ServerVerticle::handleInfoRedirect` |
 `/close-query` | `POST` | `CloseQueryHandler` |
 `/clusterStatus` | `GET` | `handleClusterStatusRequest` |
 `/healthcheck` | `GET` | `handleHealthcheckRequest` |
 `/heartbeat` | `POST` | `handleHeartbeatRequest` |
 `/info` | `GET` | [handleInfoRequest](#handleInfoRequest) |
 `/inserts-stream` | `POST` | `InsertsStreamHandler` |
 `/is_valid_property/:property` | `GET` | `handleIsValidPropertyRequest` |
 `/ksql` | `POST` | [handleKsqlRequest](#handleKsqlRequest) |
 `/ksql/terminate` | `POST` | `handleTerminateRequest` |
 `/lag` | `POST` | `handleLagReportRequest` |
 `/query` | `POST` | `handleQueryRequest` |
 `/query-stream` | `POST` | `QueryStreamHandler` |
 `/status/:type/:entity/:action` | `GET` | `handleStatusRequest` |
 `/status` | `GET` | [handleAllStatusesRequest](#handleAllStatusesRequest) |
 `/v1/metadata` | `GET` | `handleServerMetadataRequest` |
 `/v1/metadata/id` | `GET` | `handleServerMetadataClusterIdRequest` |
 `/ws/query` | `GET` | [handleWebsocket](#handleWebsocket) | <li>`application/vnd.ksql.v1+json`</li><li>`application/json`</li>

### <span id="handleAllStatusesRequest"> handleAllStatusesRequest

```java
void handleAllStatusesRequest(
  RoutingContext routingContext)
```

`handleAllStatusesRequest` requests the [Endpoints](#endpoints) to [executeAllStatuses](Endpoints.md#executeAllStatuses).

```console
$ http http://localhost:8088/status
HTTP/1.1 200 OK
content-length: 69
content-type: application/json

{
    "commandStatuses": {
        "stream/`KSQL_PROCESSING_LOG`/create": "SUCCESS"
    }
}
```

### <span id="handleInfoRequest"> handleInfoRequest

```java
void handleInfoRequest(
  RoutingContext routingContext)
```

`handleInfoRequest` requests the [Endpoints](#endpoints) to [executeInfo](Endpoints.md#executeInfo).

```console
$ http http://localhost:8088/info
HTTP/1.1 200 OK
content-length: 133
content-type: application/json

{
    "KsqlServerInfo": {
        "kafkaClusterId": "kI5f7xZWQaynAgoptiVXJw",
        "ksqlServiceId": "default_",
        "serverStatus": "RUNNING",
        "version": "0.27.2"
    }
}
```

### <span id="handleKsqlRequest"> handleKsqlRequest

```java
void handleKsqlRequest(
  RoutingContext routingContext)
```

`handleKsqlRequest` requests the [Endpoints](#endpoints) to [execute the KsqlRequest](Endpoints.md#executeKsqlRequest).

```console
$ http http://localhost:8088/ksql ksql="LIST STREAMS;"
HTTP/1.1 200 OK
content-length: 224
content-type: application/json

[
    {
        "@type": "streams",
        "statementText": "LIST STREAMS;",
        "streams": [
            {
                "isWindowed": false,
                "keyFormat": "KAFKA",
                "name": "KSQL_PROCESSING_LOG",
                "topic": "default_ksql_processing_log",
                "type": "STREAM",
                "valueFormat": "JSON"
            }
        ],
        "warnings": []
    }
]
```

### <span id="handleWebsocket"> handleWebsocket

```java
void handleWebsocket(
  RoutingContext routingContext)
```

`handleWebsocket` requests the [Endpoints](#endpoints) to [executeWebsocketStream](Endpoints.md#executeWebsocketStream).

---

`handleWebsocket` is used to handle [GET /ws/query](#setupRouter) HTTP requests.
