# BlockingQueryPublisher

`BlockingQueryPublisher` is a [QueryPublisher](QueryPublisher.md) that is used by [QueryEndpoint](../rest/QueryEndpoint.md) to [handle the result of pull or push queries](../rest/QueryEndpoint.md#createQueryPublisher).

## Creating Instance

`BlockingQueryPublisher` takes the following to be created:

* <span id="ctx"> `Context` ([Vert.x]({{ vertx.api }}/io/vertx/core/Context.html))
* <span id="workerExecutor"> `WorkerExecutor` ([Vert.x]({{ vertx.api }}/io/vertx/core/WorkerExecutor.html))

`BlockingQueryPublisher` is created when:

* `QueryEndpoint` is requested to [create a QueryPublisher](../rest/QueryEndpoint.md#createQueryPublisher) (for the result of pull or push queries)

## <span id="isPullQuery"> isPullQuery

```java
boolean isPullQuery()
```

`isPullQuery` is part of the [QueryPublisher](QueryPublisher.md#isPullQuery) abstraction.

---

`isPullQuery` flag is set in [setQueryHandle](#setQueryHandle) to indicate whether the query is a pull query (`true`) or a push query (`false`).

## <span id="setQueryHandle"> setQueryHandle

```java
void setQueryHandle(
  QueryHandle queryHandle,
  boolean isPullQuery,
  boolean isScalablePushQuery)
```

`setQueryHandle`...FIXME

---

`setQueryHandle` is used when:

* `QueryEndpoint` is requested to [create a QueryPublisher](../rest/QueryEndpoint.md#createQueryPublisher) (for the result of pull or push queries)

## Logging

Enable `ALL` logging level for `io.confluent.ksql.api.impl.BlockingQueryPublisher` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.io.confluent.ksql.api.impl.BlockingQueryPublisher=ALL
```

Refer to [Logging](../logging.md).
