# KsqlRestApplication

`KsqlRestApplication` is a ksqlDB API server (that can be started using [ksql-server-start](KsqlServerMain.md#ksql-server-start) shell script).

## Creating Instance

`KsqlRestApplication` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](../ServiceContext.md)
* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="restConfig"> [KsqlRestConfig](KsqlRestConfig.md)
* <span id="commandRunner"> [CommandRunner](CommandRunner.md)
* <span id="commandStore"> [CommandStore](CommandStore.md)
* <span id="statusResource"> `StatusResource`
* <span id="streamedQueryResource"> `StreamedQueryResource`
* <span id="ksqlResource"> [KsqlResource](KsqlResource.md)
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

## <span id="configurables"> KsqlConfigurables

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
```

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

`buildApplication` builds the name of the command internal topic.

`buildApplication` [creates a CommandStore](CommandStore.md#create).

`buildApplication`...FIXME

`buildApplication` creates a [QueryExecutor](QueryExecutor.md).

`buildApplication`...FIXME

In the end, `buildApplication` creates a [KsqlRestApplication](#creating-instance).

`buildApplication` is used when:

* `KsqlServerMain` is requested for an [Executable](KsqlServerMain.md#createExecutable)

## <span id="startAsync"> startAsync

```java
void startAsync()
```

`startAsync` prints out the following DEBUG message to the logs:

```text
Starting the ksqlDB API server
```

`startAsync`...FIXME

In the end, `startAsync` prints out the following INFO message to the logs followed by [displayWelcomeMessage](#displayWelcomeMessage).

```text
ksqlDB API server listening on [comma-separated listeners]
```

---

`startAsync` is part of the [Executable](Executable.md#startAsync) abstraction.

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

`startKsql`...FIXME

### <span id="initialize"> initialize

```java
void initialize(
  KsqlConfig configWithApplicationServer)
```

`initialize`...FIXME

### <span id="registerCommandTopic"> registerCommandTopic

```java
void registerCommandTopic()
```

`registerCommandTopic`...FIXME
