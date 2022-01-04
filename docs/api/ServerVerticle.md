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

`start` creates a [ConnectionQueryManager](#connectionQueryManager).

`start` creates an `HttpServer` ([Vert.x]({{ vertx.api }}/io/vertx/core/Vertx.html#createHttpServer-io.vertx.core.http.HttpServerOptions-)) (with the [HttpServerOptions](#httpServerOptions)) and registers the [request handlers](#setupRouter).

`start` is part of the `Verticle` ([Vert.x]({{ vertx.api }}/io/vertx/core/Verticle.html#start-io.vertx.core.Promise-)) abstraction.

### <span id="setupRouter"> setupRouter

```java
Router setupRouter()
```

`setupRouter` registers the query handlers.

URI      | HTTP Method | Handler
---------|-------------|---------
 `/` | `GET` | `ServerVerticle::handleInfoRedirect`
 `/close-query` | `POST` | `CloseQueryHandler`
 `/clusterStatus` | `GET` | `this::handleClusterStatusRequest`
 `/healthcheck` | `GET` | `this::handleHealthcheckRequest`
 `/heartbeat` | `POST` | `this::handleHeartbeatRequest`
 `/info` | `GET` | `this::handleInfoRequest`
 `/inserts-stream` | `POST` | `InsertsStreamHandler`
 `/is_valid_property/:property` | `GET` | `this::handleIsValidPropertyRequest`
 `/ksql` | `POST` | `this::handleKsqlRequest`
 `/ksql/terminate` | `POST` | `this::handleTerminateRequest`
 `/lag` | `POST` | `this::handleLagReportRequest`
 `/query` | `POST` | `this::handleQueryRequest`
 `/query-stream` | `POST` | `QueryStreamHandler`
 `/status/:type/:entity/:action` | `GET` | `this::handleStatusRequest`
 `/status` | `GET` | `this::handleAllStatusesRequest`
 `/v1/metadata` | `GET` | `this::handleServerMetadataRequest`
 `/v1/metadata/id` | `GET` | `this::handleServerMetadataClusterIdRequest`
 `/ws/query` | `GET` | `this::handleWebsocket`
