# KsqlServerMain

`KsqlServerMain` is a [standalone command-line application](#main) to [start one of the Executables](#createExecutable):

1. [StandaloneExecutor](StandaloneExecutor.md) for [queries file](ServerOptions.md#getQueriesFile)
1. [KsqlRestApplication](KsqlRestApplication.md) unless [ksql.connect.worker.config](../KsqlConfig.md#CONNECT_WORKER_CONFIG_FILE_PROPERTY) configuration property is specified
1. `MultiExecutable` with a `ConnectExecutable` and the `KsqlRestApplication`

`KsqlServerMain` supports [command-line options](ServerOptions.md).

`KsqlServerMain` can be launched on command line using [ksql-server-start](index.md) shell script.

## Creating Instance

`KsqlServerMain` takes the following to be created:

* <span id="executable"> [Executable](Executable.md)
* <span id="shutdownHandler"> Shutdown Handler

`KsqlServerMain` is created when:

* `KsqlServerMain` application is [launched](#main)

## <span id="main"> Launching Application

`main` [parses the command-line options](ServerOptions.md#parse) and loads the required [properties file](ServerOptions.md#getPropertiesFile) (with the Java system properties applied overriding earlier values).

`main` creates and [validates](#validateConfig) a [KsqlConfig](../KsqlConfig.md).

`main` [configures QueryLogger](../QueryLogger.md#configure) (with the `KsqlConfig`).

`main` [creates an Executable](#createExecutable) based on the following:

1. Properties with the Java system properties applied
1. [queries file](ServerOptions.md#getQueriesFile) command-line option (if defined)
1. `ksql.server.install.dir` configuration property from the properties file
1. A new [KsqlConfig](../KsqlConfig.md) with the config and system properties
1. A new `MetricCollectors`

`main` creates a new [KsqlServerMain](#creating-instance) (with the `Executable`) and [starts it up](#tryStartApp).

!!! note
    `main` is paused when [starting up the executable](#tryStartApp) (using [awaitTerminated](Executable.md#awaitTerminated)) until [notifyTerminated](Executable.md#notifyTerminated) which happens as part of a Java Virtual Machine shutdown hook.

### <span id="createExecutable"> Creating Executable

```java
Executable createExecutable(
  Map<String, String> properties,
  Optional<String> queriesFile,
  String installDir,
  KsqlConfig ksqlConfig,
  MetricCollectors metricCollectors)
```

With [queries file](ServerOptions.md#getQueriesFile) specified, `createExecutable` returns a new [StandaloneExecutor](StandaloneExecutorFactory.md#create).

Otherwise, `createExecutable` creates a [KsqlRestConfig](KsqlRestConfig.md) (with the given `properties`) to [build a KsqlRestApplication](KsqlRestApplication.md#buildApplication) (with the `KsqlRestConfig` and the given `MetricCollectors`).

With no [ksql.connect.worker.config](../KsqlConfig.md#CONNECT_WORKER_CONFIG_FILE_PROPERTY) configuration property specified, `createExecutable` returns the `KsqlRestApplication`. Otherwise, `createExecutable` creates a `ConnectExecutable` and returns a `MultiExecutable` (with the `ConnectExecutable` and the `KsqlRestApplication`).

### <span id="tryStartApp"> tryStartApp

```java
void tryStartApp()
```

`tryStartApp` prints out the following INFO message to the logs:

```text
Starting server
```

`tryStartApp` requests the [Executable](#executable) to [startAsync](Executable.md#startAsync).

`tryStartApp` prints out the following INFO message to the logs:

```text
Server up and running
```

`tryStartApp` requests the [Executable](#executable) to [awaitTerminated](Executable.md#awaitTerminated).

Finally (when the [Executable](#executable) was requested to terminate), `tryStartApp` prints out the following INFO message to the logs:

```text
Server shutting down
```

`tryStartApp` requests the [Executable](#executable) to [shutdown](Executable.md#shutdown).
