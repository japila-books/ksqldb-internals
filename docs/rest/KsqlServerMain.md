# KsqlServerMain

`KsqlServerMain` is a [standalone (command-line) application](#main) with [command-line options](ServerOptions.md):

```text
$ ./bin/ksql-server-start --help
NAME
        server - KSQL Cluster

SYNOPSIS
        server [ {-h | --help} ] [ --queries-file <queriesFile> ] [--]
                <config-file>

OPTIONS
        -h, --help
            Display help information

        --queries-file <queriesFile>
            Path to the query file on the local machine.

        --
            This option can be used to separate command-line options from the
            list of arguments (useful when arguments might be mistaken for
            command-line options)

        <config-file>
            A file specifying configs for the KSQL Server, KSQL, and its
            underlying Kafka Streams instance(s). Refer to KSQL documentation
            for a list of available configs.

            This option may occur a maximum of 1 times
```

## <span id="ksql-server-start"> ksql-server-start Shell Script

`KsqlServerMain` can be launched using `ksql-server-start` shell script (or `ksql-run-class` directly).

```text
./bin/ksql-run-class io.confluent.ksql.rest.server.KsqlServerMain
```

## Creating Instance

`KsqlServerMain` takes the following to be created:

* <span id="executable"> [Executable](Executable.md)
* <span id="shutdownHandler"> Shutdown Handler

`KsqlServerMain` is created when:

* `KsqlServerMain` standalone application is [launched](#main)

## <span id="main"> Launching KsqlServerMain

`main` [parses the command-line options](ServerOptions.md#parse) and loads the required [properties file](ServerOptions.md#getPropertiesFile).

`main` takes `ksql.server.install.dir` configuration property from the properties file.

`main` creates and [validates](#validateConfig) a [KsqlConfig](../KsqlConfig.md).

`main` [configures QueryLogger](../QueryLogger.md#configure).

`main` takes the [queries file](ServerOptions.md#getQueriesFile) (if used) and [creates an Executable](#createExecutable).

`main` creates a new [KsqlServerMain](#creating-instance) (with the `Executable`) and [starts it up](#tryStartApp).

!!! note
    `main` is paused when [starting up the executable](#tryStartApp) (using [awaitTerminated](Executable.md#awaitTerminated)) until [notifyTerminated](Executable.md#notifyTerminated) which happens as part of a Java Virtual Machine shutdown hook.

### <span id="createExecutable"> Creating Executable

```java
Executable createExecutable(
  Map<String, String> properties,
  Optional<String> queriesFile,
  String installDir,
  KsqlConfig ksqlConfig)
```

With [queries file](ServerOptions.md#getQueriesFile) specified, `createExecutable` [creates](StandaloneExecutorFactory.md#create) and returns a [StandaloneExecutor](StandaloneExecutor.md).

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
