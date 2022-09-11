# KsqlResource

`KsqlResource` is used by [KsqlRestApplication](KsqlRestApplication.md#ksqlResource) to execute statements (indirectly using [KsqlServerEndpoints](KsqlServerEndpoints.md#ksqlResource)).

## Creating Instance

`KsqlResource` takes the following to be created:

* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* [CommandRunner](#commandRunner)
* <span id="distributedCmdResponseTimeout"> `distributedCmdResponse` Timeout
* <span id="activenessRegistrar"> `ActivenessRegistrar`
* <span id="injectorFactory"> [Injector](../Injector.md) factory
* <span id="authorizationValidator"> `KsqlAuthorizationValidator`
* <span id="errorHandler"> `Errors` Handlers
* <span id="denyListPropertyValidator"> `DenyListPropertyValidator`
* <span id="commandRunnerWarning"> CommandRunner warning message

`KsqlResource` is created when:

* `KsqlRestApplication` utility is used to [build a KsqlRestApplication](KsqlRestApplication.md#buildApplication) (and creates a [KsqlRestApplication](KsqlRestApplication.md#ksqlResource))

### <span id="commandRunner"> CommandRunner

`KsqlResource` is given a [CommandRunner](CommandRunner.md) when [created](#creating-instance).

The `CommandRunner` is used only to access the [CommandQueue](CommandRunner.md#getCommandQueue) when:

* [configure](#configure) (to create a [DistributingExecutor](DistributingExecutor.md) and a [DefaultCommandQueueSync](DefaultCommandQueueSync.md) for [RequestHandler](RequestHandler.md))
* [handleKsqlStatements](#handleKsqlStatements) (to [httpWaitForCommandSequenceNumber](CommandStoreUtil.md#httpWaitForCommandSequenceNumber))

## <span id="configure"> configure

```java
void configure(
  KsqlConfig config)
```

`configure` is part of the [KsqlConfigurable](KsqlConfigurable.md#configure) abstraction.

---

`configure`...FIXME

## <span id="shouldSynchronize"> shouldSynchronize

```java
boolean shouldSynchronize(
  Class<? extends Statement> statementClass)
```

`shouldSynchronize` is `true` when the given `statementClass` is as follows:

1. Not in [SYNC_BLACKLIST](#SYNC_BLACKLIST)
1. In [EXECUTOR_MAP](CustomExecutors.md#EXECUTOR_MAP)

---

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

## <span id="handleKsqlStatements"> Handling KSQL Statements

```java
EndpointResponse handleKsqlStatements(
  KsqlSecurityContext securityContext,
  KsqlRequest request)
```

`handleKsqlStatements` prints out the following INFO message to the logs:

```text
Received: [request]
```

`handleKsqlStatements` requests the [KsqlEngine](#ksqlEngine) to [parse the KSQL text](../KsqlEngine.md#parse) (from the given `KsqlRequest`).

`handleKsqlStatements` requests the [RequestValidator](#validator) to [validate](RequestValidator.md#validate) the statements (in a `SandboxedServiceContext`).

`handleKsqlStatements` requests the [RequestHandler](#handler) to [execute the KSQL statements](RequestHandler.md#execute).

In the end, `handleKsqlStatements` prints out the following INFO message to the logs:

```text
Processed successfully: [request]
```

---

`handleKsqlStatements` is used when:

* `KsqlServerEndpoints` is requested to [execute a KsqlRequest](KsqlServerEndpoints.md#executeKsqlRequest)
* `ServerInternalKsqlClient` is requested to `makeKsqlRequest`

## Logging

Enable `ALL` logging level for `io.confluent.ksql.rest.server.resources.KsqlResource` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.io.confluent.ksql.rest.server.resources.KsqlResource=ALL
```

Refer to [Logging](../logging.md).
