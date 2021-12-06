# KsqlRestApplication

`KsqlRestApplication` is a ksqlDB API server.

## Creating Instance

`KsqlRestApplication` takes the following to be created:

* <span id="serviceContext"> ServiceContext
* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="restConfig"> KsqlRestConfig
* <span id="commandRunner"> CommandRunner
* <span id="commandStore"> CommandStore
* <span id="statusResource"> StatusResource
* <span id="streamedQueryResource"> StreamedQueryResource
* <span id="ksqlResource"> KsqlResource
* <span id="versionCheckerAgent"> VersionCheckerAgent
* <span id="ksqlSecurityContextProvider"> KsqlSecurityContextProvider
* <span id="securityExtension"> KsqlSecurityExtension
* <span id="authenticationPlugin"> AuthenticationPlugin
* <span id="serverState"> ServerState
* <span id="processingLogContext"> ProcessingLogContext
* <span id="preconditions"> `KsqlServerPrecondition`s
* <span id="configurables"> `KsqlConfigurable`s
* <span id="rocksDBConfigSetterHandler"> `Consumer<KsqlConfig>`
* <span id="heartbeatAgent"> `HeartbeatAgent`
* <span id="lagReportingAgent"> `LagReportingAgent`
* <span id="vertx"> Vertx
* <span id="denyListPropertyValidator"> DenyListPropertyValidator
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="scalablePushQueryMetrics"> `ScalablePushQueryMetrics`
* <span id="localCommands"> `LocalCommands`
* <span id="queryExecutor"> QueryExecutor

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

`KsqlRestApplication` is created when:

* `KsqlRestApplication` utility is used to [buildApplication](#buildApplication)

## <span id="buildApplication"> buildApplication

```java
KsqlRestApplication buildApplication(
  KsqlRestConfig restConfig)
KsqlRestApplication buildApplication(
  String metricsPrefix,
  KsqlRestConfig restConfig,
  Function<Supplier<Boolean>, VersionCheckerAgent> versionCheckerFactory,
  int maxStatementRetries,
  ServiceContext serviceContext,
  Supplier<SchemaRegistryClient> schemaRegistryClientFactory,
  Vertx vertx,
  KsqlClient sharedClient)
```

`buildApplication`...FIXME

`buildApplication` is used when:

* `KsqlServerMain` is requested to [createExecutable](KsqlServerMain.md#createExecutable)

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
