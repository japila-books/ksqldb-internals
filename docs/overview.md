# {{ book.title }}

[ksqlDB](https://ksqldb.io/) is _"the database purpose-built for stream processing applications._"

## How ksqlDB Works

ksqlDB can be used in the following execution modes:

* [Embedded](embedded/index.md)
* [Headless](headless/index.md)
* [REST](rest/index.md)

The primary ksqlDB services are [KsqlEngine](KsqlEngine.md) and [ServiceContext](ServiceContext.md).

ksqlDB uses [KsqlServerMain](rest/KsqlServerMain.md) to handle SQL queries (from the [command line](rest/ServerOptions.md#queries-file) or sent through a REST endpoint, e.g. using [ksql](cli/Ksql.md)).

### Executing DDL Commands

[DdlCommand](DdlCommand.md)s (e.g. [CREATE STREAM](parser/CreateStream.md)) are parsed by [AstBuilder.Visitor](parser/AstBuilder_Visitor.md) to [Statement](parser/Statement.md)s.

DDL commands are then planned for execution using [EngineExecutor](EngineExecutor.md#plan) and executed by (per command-line options):

* [DistributingExecutor](rest/DistributingExecutor.md#execute)
* [StatementExecutor](rest/StatementExecutor.md#handleExecutableDdl)

`DistributingExecutor` uses a transactional Kafka producer to [enqueue the command](rest/CommandQueue.md#enqueueCommand) (to the [CommandQueue](#commandQueue)) that is then fetched by [CommandRunner](rest/CommandRunner.md#fetchAndRunCommands).

[CommandRunner](rest/CommandRunner.md) uses [InteractiveStatementExecutor](rest/InteractiveStatementExecutor.md) to [execute commands](rest/CommandRunner.md#executeStatement).

When requested to [handle a statement](rest/InteractiveStatementExecutor.md#handleStatement) (as part of an enqueued command), `InteractiveStatementExecutor` uses [KsqlEngine](KsqlEngine.md) to [execute a ksql plan](KsqlEngine.md#execute).

For [DdlCommand](DdlCommand.md)s, [EngineExecutor](EngineExecutor.md) uses [EngineContext](EngineContext.md) to [execute it](EngineContext.md#executeDdl) (using [DdlCommandExec](DdlCommandExec.md#execute)).

## Run It Yourself

!!! note "Get standalone ksqlDB first"
    The following assumes the standalone ksqlDB version installed per [ksqlDB Quickstart](https://ksqldb.io/quickstart-standalone-tarball.html#quickstart-content).

    ```text
    curl http://ksqldb-packages.s3.amazonaws.com/archive/{{ ksqldb.version_major }}/confluent-ksqldb-{{ ksqldb.version }}.tar.gz \
        --output confluent-ksqldb-{{ ksqldb.version }}.tar.gz
    ```

```console
$ cd $KAFKA_HOME

$ ./bin/zookeeper-server-start.sh config/zookeeper.properties

$ ./bin/kafka-server-start.sh config/server.properties

$ ./bin/ksql-server-start etc/ksqldb/ksql-server.properties

                  ===========================================
                  =       _              _ ____  ____       =
                  =      | | _____  __ _| |  _ \| __ )      =
                  =      | |/ / __|/ _` | | | | |  _ \      =
                  =      |   <\__ \ (_| | | |_| | |_) |     =
                  =      |_|\_\___/\__, |_|____/|____/      =
                  =                   |_|                   =
                  =        The Database purpose-built       =
                  =        for stream processing apps       =
                  ===========================================

Copyright 2017-2022 Confluent Inc.

Server 0.27.2 listening on http://0.0.0.0:8088

To access the KSQL CLI, run:
ksql http://0.0.0.0:8088
```

ksqlDB uses [ksql](cli/Ksql.md) as the command-line interactive environment.

```console
$ ./bin/ksql
                  ===========================================
                  =       _              _ ____  ____       =
                  =      | | _____  __ _| |  _ \| __ )      =
                  =      | |/ / __|/ _` | | | | |  _ \      =
                  =      |   <\__ \ (_| | | |_| | |_) |     =
                  =      |_|\_\___/\__, |_|____/|____/      =
                  =                   |_|                   =
                  =        The Database purpose-built       =
                  =        for stream processing apps       =
                  ===========================================

Copyright 2017-2022 Confluent Inc.

CLI v0.27.2, Server v0.27.2 located at http://localhost:8088
Server Status: RUNNING

Having trouble? Type 'help' (case-insensitive) for a rundown of how things work!

ksql>
```

## Learning Resources

### Articles

1. [Deep Dive into ksqlDB Deployment Options](https://www.confluent.io/blog/deep-dive-ksql-deployment-options/)

### Videos

1. [KSQL 201: A Deep Dive into Query Processing](https://www.confluent.io/kafka-summit-london18/ksql-201-a-deep-dive-into-query-processing/)
