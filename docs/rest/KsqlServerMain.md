# KsqlServerMain

`KsqlServerMain` is a [standalone (command-line) application](#main).

## Creating Instance

`KsqlServerMain` takes the following to be created:

* <span id="executable"> [Executable](Executable.md)
* <span id="shutdownHandler"> Shutdown Handler

`KsqlServerMain` is created when:

* `KsqlServerMain` standalone application is [launched](#main)

## <span id="main"> Launching KsqlServerMain

`main` [creates a ServerOptions from the command-line arguments](ServerOptions.md#parse).

`main`...FIXME

### <span id="createExecutable"> Creating Executable

```java
Executable createExecutable(
  Map<String, String> properties,
  Optional<String> queriesFile,
  String installDir,
  KsqlConfig ksqlConfig)
```

With [queriesFile](ServerOptions.md#getQueriesFile) specified, `createExecutable` [creates a StandaloneExecutor](StandaloneExecutorFactory.md#create).

Otherwise, `createExecutable`...FIXME

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
