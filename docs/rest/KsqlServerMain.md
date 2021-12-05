# KsqlServerMain

`KsqlServerMain` is a [standalone (command-line) application](#main).

## Creating Instance

`KsqlServerMain` takes the following to be created:

* <span id="executable"> `Executable`
* <span id="shutdownHandler"> Shutdown Handler

`KsqlServerMain` is created when:

* `KsqlServerMain` standalone application is launched

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
