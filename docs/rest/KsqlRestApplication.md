# KsqlRestApplication

`KsqlRestApplication` is a ksqlDB REST API server.

`KsqlRestApplication` uses [CommandRunner](#commandRunner) to [process prior commands](CommandRunner.md#processPriorCommands) and then [run new commands](CommandRunner.md#fetchAndRunCommands) continuously (off a [CommandQueue](CommandRunner.md#commandStore)).

`KsqlRestApplication` can be started using [ksql-server-start](index.md) shell script.

## Creating Instance

`KsqlRestApplication` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](../ServiceContext.md)
* [KsqlEngine](#ksqlEngine)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="restConfig"> [KsqlRestConfig](KsqlRestConfig.md)
* [CommandRunner](#commandRunner)
* <span id="commandStore"> [CommandStore](CommandStore.md)
* <span id="statusResource"> `StatusResource`
* <span id="streamedQueryResource"> `StreamedQueryResource`
* [KsqlResource](#ksqlResource)
* <span id="versionCheckerAgent"> [VersionCheckerAgent](../VersionCheckerAgent.md)
* <span id="ksqlSecurityContextProvider"> `KsqlSecurityContextProvider`
* <span id="securityExtension"> `KsqlSecurityExtension`
* <span id="authenticationPlugin"> `AuthenticationPlugin`
* <span id="serverState"> `ServerState`
* <span id="processingLogContext"> `ProcessingLogContext`
* <span id="preconditions"> `KsqlServerPrecondition`s
* [KsqlConfigurables](#configurables)
* <span id="rocksDBConfigSetterHandler"> RocksDB Config Setter Handler (Function of [KsqlConfig](../KsqlConfig.md))
* <span id="heartbeatAgent"> `HeartbeatAgent`
* <span id="lagReportingAgent"> `LagReportingAgent`
* <span id="vertx"> Vert.x
* <span id="denyListPropertyValidator"> `DenyListPropertyValidator`
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="scalablePushQueryMetrics"> `ScalablePushQueryMetrics`
* <span id="localCommands"> `LocalCommands`
* <span id="queryExecutor"> [QueryExecutor](QueryExecutor.md)

---

When created, `KsqlRestApplication` prints out the following DEBUG message to the logs:

```text
Creating instance of ksqlDB API server
```

In the end, `KsqlRestApplication` prints out the following DEBUG message to the logs:

```text
ksqlDB API server instance created
```

---

`KsqlRestApplication` is created using [buildApplication](#buildApplication) utility.

### <span id="ksqlEngine"> KsqlEngine

`KsqlRestApplication` is given a [KsqlEngine](../KsqlEngine.md) when [created](#creating-instance).

The `KsqlEngine` is active until [shutdown](#shutdown).

The `KsqlEngine` is used in [startAsync](#startAsync) to create the REST API endpoints:

* Create [WSQueryEndpoint](WSQueryEndpoint.md#ksqlEngine) (along with [StatementParser](StatementParser.md))
* Create [KsqlServerEndpoints](KsqlServerEndpoints.md#ksqlEngine) for [API server](#apiServer)

### <span id="ksqlResource"> KsqlResource

`KsqlRestApplication` is given a [KsqlResource](KsqlResource.md) when [created](#creating-instance).

`KsqlResource` is used to create a [KsqlServerEndpoints](KsqlServerEndpoints.md#ksqlResource) upon [starting](#startAsync).

`KsqlResource` is also used to [maybeCreateProcessingLogStream](#maybeCreateProcessingLogStream) upon [initializing](#initialize).

### <span id="commandRunner"> CommandRunner

`KsqlRestApplication` is given a [CommandRunner](CommandRunner.md) when [created](#creating-instance).

The `CommandRunner` is requested to [processPriorCommands](CommandRunner.md#processPriorCommands) before [starting command execution](CommandRunner.md#start) in [initialize](#initialize). The `CommandRunner` is up and running until [shutdown](#shutdown) (when it is requested to [close](CommandRunner.md#close)).

The `CommandRunner` is used to create a [HealthCheckResource](#healthCheckResource) when `KsqlRestApplication` is [created](#creating-instance).

### <span id="configurables"> KsqlConfigurables

`KsqlRestApplication` is given [KsqlConfigurable](KsqlConfigurable.md)s when [created](#creating-instance).

All the given [KsqlConfigurable](KsqlConfigurable.md)s are also given separately to create the `KsqlRestApplication`:

* [KsqlResource](#ksqlResource)
* [StreamedQueryResource](#streamedQueryResource)
* `InteractiveStatementExecutor` (that is part of the [StatusResource](#statusResource))

`KsqlConfigurable`s are requested to [configure](KsqlConfigurable.md#configure) (with a [KsqlConfig with an application.server property assigned](#buildConfigWithPort)) in [startAsync](#startAsync).

## <span id="buildApplication"> Building KsqlRestApplication

```java
KsqlRestApplication buildApplication(
  KsqlRestConfig restConfig,
  MetricCollectors metricCollectors)
KsqlRestApplication buildApplication(
  String metricsPrefix,
  KsqlRestConfig restConfig,
  ServerState serverState,
  Function<Supplier<Boolean>, VersionCheckerAgent> versionCheckerFactory,
  int maxStatementRetries,
  ServiceContext serviceContext,
  Supplier<SchemaRegistryClient> schemaRegistryClientFactory,
  ConnectClientFactory connectClientFactory,
  Vertx vertx,
  KsqlClient sharedClient,
  DefaultServiceContextFactory defaultServiceContextFactory,
  UserServiceContextFactory userServiceContextFactory,
  MetricCollectors metricCollectors)
```

`buildApplication` is used when:

* `KsqlServerMain` is requested for an [Executable](KsqlServerMain.md#createExecutable)

### <span id="buildApplication-vertx"> Step 1. Vert.x

`buildApplication` creates a Vert.x subsystem.

!!! note "Vert.x"
    [Vert.x](https://vertx.io/) allows writing reactive applications on the JVM with support for HTTP, TCP, UDP, file system, asynchronous streams. etc.

`buildApplication` [creates an internal KsqlClient](InternalKsqlClientFactory.md#createInternalClient).

`buildApplication` creates a `KsqlSchemaRegistryClientFactory` and `DefaultConnectClientFactory`.

`buildApplication` determines the Kafka Cluster ID and reads the [ksql.service.id](../KsqlConfig.md#KSQL_SERVICE_ID_CONFIG) configuration property (from the [KsqlConfig](../KsqlConfig.md)).

`buildApplication` creates a [KsqlRestConfig](KsqlRestConfig.md).

`buildApplication`...FIXME

`buildApplication` creates a [KsqlEngine](../KsqlEngine.md).

`buildApplication`...FIXME

`buildApplication` [builds the name of the command topic](ReservedInternalTopics.md#commandTopic) (to create a [CommandStore](CommandStore.md#create) and a [CommandRunner](CommandRunner.md#commandTopicName)).

`buildApplication` [creates a CommandStore](CommandStore.md#create).

### <span id="buildApplication-statementExecutor"> Step x. InteractiveStatementExecutor

`buildApplication` [creates an InteractiveStatementExecutor](InteractiveStatementExecutor.md).

`buildApplication`...FIXME

`buildApplication` creates a [QueryExecutor](QueryExecutor.md) and a `StreamedQueryResource`.

### <span id="buildApplication-commandRunner"> Step x. CommandRunner

`buildApplication` creates a [CommandRunner](CommandRunner.md) (with the [InteractiveStatementExecutor](#buildApplication-statementExecutor)).

`buildApplication`...FIXME

### <span id="buildApplication-KsqlRestApplication"> Step x. KsqlRestApplication

In the end, `buildApplication` creates a [KsqlRestApplication](#creating-instance).

## <span id="startAsync"> startAsync

```java
void startAsync()
```

`startAsync` is part of the [Executable](Executable.md#startAsync) abstraction.

---

`startAsync` prints out the following DEBUG message to the logs:

```text
Starting the ksqlDB API server
```

`startAsync` [creates a ServerMetadataResource](#serverMetadataResource).

`startAsync` creates a [StatementParser](StatementParser.md) (with the [KsqlEngine](#ksqlEngine)).

`startAsync`...FIXME

`startAsync` creates a [Server](../api/Server.md) and [starts it](../api/Server.md#start).

`startAsync`...FIXME

In the end, `startAsync` prints out the following INFO message to the logs followed by [displayWelcomeMessage](#displayWelcomeMessage).

```text
ksqlDB API server listening on [comma-separated listeners]
```

### <span id="displayWelcomeMessage"> displayWelcomeMessage

```java
void displayWelcomeMessage()
```

`displayWelcomeMessage`...FIXME

In the end, `displayWelcomeMessage` prints out the following to the standard output:

```text
Server [version] listening on [comma-separated listeners]

To access the KSQL CLI, run:
ksql [listener]
```

### <span id="startKsql"> startKsql

```java
void startKsql(
  KsqlConfig ksqlConfigWithPort)
```

`startKsql` [cleanupOldState](#cleanupOldState) before [initialization](#initialize) (with the given [KsqlConfig](../KsqlConfig.md)).

### <span id="initialize"> Initializing

```java
void initialize(
  KsqlConfig configWithApplicationServer)
```

`initialize` executes the [rocksDBConfigSetterHandler](#rocksDBConfigSetterHandler) with the [ksqlConfigNoPort](#ksqlConfigNoPort).

`initialize` [registerCommandTopic](#registerCommandTopic).

`initialize` requests the [CommandStore](#commandStore) to [start](CommandStore.md#start).

`initialize` [maybeCreateProcessingLogTopic](ProcessingLogServerUtils.md#maybeCreateProcessingLogTopic) with the following:

* [Shared KafkaTopicClient](../ServiceContext.md#getTopicClient)
* [ProcessingLogConfig](../processing-log/ProcessingLogContext.md#getConfig)
* [ksqlConfigNoPort](#ksqlConfigNoPort)

`initialize` requests the [CommandRunner](#commandRunner) to [processPriorCommands](CommandRunner.md#processPriorCommands) (with a new `PersistentQueryCleanupImpl`).

`initialize` requests the [CommandRunner](#commandRunner) to [start processing commands](CommandRunner.md#start).

`initialize` [maybeCreateProcessingLogStream](#maybeCreateProcessingLogStream).

`initialize` starts the [heartbeatAgent](#heartbeatAgent) and the [lagReportingAgent](#lagReportingAgent) agents (if specified).

In the end, `initialize` changes the [ServerState](#serverState) to `READY`.

### <span id="registerCommandTopic"> registerCommandTopic

```java
void registerCommandTopic()
```

`registerCommandTopic` requests the [CommandStore](#commandStore) for the [name of the command topic](CommandStore.md#getCommandTopicName).

`registerCommandTopic` makes sure that the internal command topic is available in the Kafka cluster and in sync with backup (if configured).

`registerCommandTopic` [creates the command topic if not exists](../KsqlInternalTopicUtils.md#ensureTopic).

## <span id="maybeCreateProcessingLogStream"> maybeCreateProcessingLogStream

```java
void maybeCreateProcessingLogStream(
  ProcessingLogConfig processingLogConfig,
  KsqlConfig ksqlConfig,
  KsqlRestConfig restConfig,
  KsqlResource ksqlResource,
  ServiceContext serviceContext)
```

`maybeCreateProcessingLogStream` does nothing and returns immediately when [ksql.logging.processing.stream.auto.create](../processing-log/ProcessingLogConfig.md#STREAM_AUTO_CREATE) is turned off (`false`).

`maybeCreateProcessingLogStream`...FIXME

## <span id="initializeHeartbeatAgent"> Initializing HeartbeatAgent

```java
Optional<HeartbeatAgent> initializeHeartbeatAgent(
  KsqlRestConfig restConfig,
  KsqlEngine ksqlEngine,
  ServiceContext serviceContext,
  Optional<LagReportingAgent> lagReportingAgent)
```

With [ksql.heartbeat.enable](KsqlRestConfig.md#KSQL_HEARTBEAT_ENABLE_CONFIG) enabled, `initializeHeartbeatAgent` builds a [HeartbeatAgent](HeartbeatAgent.md).

Otherwise, `initializeHeartbeatAgent` does nothing and returns no `HeartbeatAgent`.

## Logging

Enable `ALL` logging level for `io.confluent.ksql.rest.server.KsqlRestApplication` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.io.confluent.ksql.rest.server.KsqlRestApplication=ALL
```

Refer to [Logging](../logging.md).
