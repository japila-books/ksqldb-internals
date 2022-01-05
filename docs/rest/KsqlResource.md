# KsqlResource

## <span id="configure"> configure

```java
void configure(
  KsqlConfig config)
```

`configure`...FIXME

`configure` is part of the [KsqlConfigurable](KsqlConfigurable.md#configure) abstraction.

## <span id="shouldSynchronize"> shouldSynchronize

```java
boolean shouldSynchronize(
  Class<? extends Statement> statementClass)
```

`shouldSynchronize` is `true` when the given `statementClass` is as follows:

1. Not in [SYNC_BLACKLIST](#SYNC_BLACKLIST)
1. In [EXECUTOR_MAP](CustomExecutors.md#EXECUTOR_MAP)

`shouldSynchronize` is used when:

* `KsqlResource` is requested to [configure](#configure) (and creates a `DefaultCommandQueueSync` for the [RequestHandler](#handler))

## <span id="SYNC_BLACKLIST"> SYNC_BLACKLIST

* `ListTopics`
* `ListFunctions`
* `DescribeFunction`
* `ListProperties`
* `SetProperty`
* `UnsetProperty`

## <span id="handler"> RequestHandler

`KsqlResource` creates a [RequestHandler](RequestHandler.md) when requested to [configure](#configure).

`RequestHandler` is used when:

* [terminateCluster](#terminateCluster)
* [handleKsqlStatements](#handleKsqlStatements)

## <span id="handleKsqlStatements"> Handling Statements

```java
EndpointResponse handleKsqlStatements(
  KsqlSecurityContext securityContext,
  KsqlRequest request)
```

`handleKsqlStatements` prints out the following INFO message to the logs:

```text
Received: [request]
```

`handleKsqlStatements` requests the [KsqlEngine](#ksqlEngine) to [parse the SQL text](../KsqlEngine.md#parse) (from the given `KsqlRequest`).

`handleKsqlStatements` requests the [RequestValidator](#validator) to [validate](RequestValidator.md#validate) the statements (in a `SandboxedServiceContext`).

`handleKsqlStatements` requests the [RequestHandler](#handler) to [execute the SQL statements](RequestHandler.md#execute).

In the end, `handleKsqlStatements` prints out the following INFO message to the logs:

```text
Processed successfully: [request]
```

---

`handleKsqlStatements` is used when:

* `KsqlServerEndpoints` is requested to [execute a KsqlRequest](KsqlServerEndpoints.md#executeKsqlRequest)
* `ServerInternalKsqlClient` is requested to `makeKsqlRequest`
