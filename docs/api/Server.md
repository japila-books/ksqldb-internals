# Server

`Server` is a ksqlDB API server that [deploys ServerVerticles](#start) (on the [Vertx](#vertx) actor system).

## Creating Instance

`Server` takes the following to be created:

* <span id="vertx"> `Vertx` ([Vert.x]({{ vertx.api }}/io/vertx/core/Vertx.html))
* <span id="config"> [KsqlRestConfig](../rest/KsqlRestConfig.md)
* <span id="endpoints"> [Endpoints](Endpoints.md)
* <span id="securityExtension"> `KsqlSecurityExtension`
* <span id="authenticationPlugin"> `AuthenticationPlugin`
* <span id="serverState"> `ServerState`
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`

`Server` is created when:

* `KsqlRestApplication` is requested to [startAsync](../rest/KsqlRestApplication.md#startAsync)

## <span id="start"> Starting API Server

```java
void start()
```

`start`...FIXME

`start` takes [ksql.verticle.instances](../rest/KsqlRestConfig.md#VERTICLE_INSTANCES) configuration property (from the [KsqlRestConfig](#config)).

`start` prints out the following DEBUG message to the logs:

```text
Deploying [instances] instances of server verticle
```

`start` creates and deploys a [ServerVerticle](ServerVerticle.md) for every [listener URI](../rest/KsqlRestConfig.md#LISTENERS_CONFIG) and [ksql.verticle.instances](../rest/KsqlRestConfig.md#VERTICLE_INSTANCES) configured.

---

`start` is used when:

* `KsqlRestApplication` is requested to [startAsync](../rest/KsqlRestApplication.md#startAsync)
* `Server` is requested to [restart](#restart)

## <span id="restart"> Restarting API Server

```java
void restart()
```

`restart` prints out the following INFO message to the logs:

```text
Restarting server
```

In the end, `restart` [stops](#stop) and immediately [starts](#start) the API server.

`restart` is used when:

* `Server` is requested to [configureTlsCertReload](#configureTlsCertReload)

## Logging

Enable `ALL` logging level for `io.confluent.ksql.api.server.Server` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.io.confluent.ksql.api.server.Server=ALL
```

Refer to [Logging](../logging.md).
