# QueryStreamHandler

`QueryStreamHandler` is a `Handler` ([Vert.x]({{ vertx.api }}/io/vertx/core/Handler.html)) to handle the following REST endpoints:

* `/query`
* `/query-stream`

## Creating Instance

`QueryStreamHandler` takes the following to be created:

* <span id="endpoints"> [Endpoints](Endpoints.md)
* <span id="connectionQueryManager"> `ConnectionQueryManager`
* <span id="context"> `Context` ([Vert.x]({{ vertx.api }}/io/vertx/core/Context.html))
* <span id="server"> [Server](Server.md)
* <span id="queryCompatibilityMode"> `queryCompatibilityMode` flag

`QueryStreamHandler` is created when:

* `ServerVerticle` is requested to [setup a Router](ServerVerticle.md#setupRouter)

## <span id="handle"> Handling HTTP Request

```java
void handle(
  RoutingContext routingContext)
```

`handle` is part of the `Handler` ([Vert.x]({{ vertx.api }}/io/vertx/core/Handler.html#handle-E-)) abstraction.

---

`handle` requests the [Endpoints](#endpoints) to [createQueryPublisher](Endpoints.md#createQueryPublisher) and then [handleQueryPublisher](#handleQueryPublisher).

### <span id="handleQueryPublisher"> handleQueryPublisher

```java
void handleQueryPublisher(
  RoutingContext routingContext,
  QueryPublisher queryPublisher,
  MetricsCallbackHolder metricsCallbackHolder,
  long startTimeNanos)
```

For a pull query, `handleQueryPublisher`...FIXME

For a scalable push query, `handleQueryPublisher`...FIXME

Otherwise, `handleQueryPublisher`...FIXME

In the end, `handleQueryPublisher` requests the given `QueryPublisher` to `subscribe` to a new `QuerySubscriber`.

## Logging

Enable `ALL` logging level for `io.confluent.ksql.api.server.QueryStreamHandler` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.io.confluent.ksql.api.server.QueryStreamHandler=ALL
```

Refer to [Logging](../logging.md).
