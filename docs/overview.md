# {{ book.title }}

[ksqlDB](https://ksqldb.io/) is _"the database purpose-built for stream processing applications._"

## How ksqlDB Works

ksqlDB can be used in the following execution modes:

* [Embedded](embedded/index.md)
* [Headless](headless/index.md)
* [REST](rest/index.md)

The primary ksqlDB services are [KsqlEngine](KsqlEngine.md) and [ServiceContext](ServiceContext.md).

KSQL statements (e.g., DDL commands and queries) can be executed from the following:

* [Query file](rest/ServerOptions.md#queries-file) ([headless](headless/index.md) mode)
* [ksql](cli/Ksql.md) ([REST](rest/index.md) mode)

KSQL statements are parsed by [AstBuilder.Visitor](parser/AstBuilder.Visitor.md) to [Statement](parser/Statement.md)s.

## Executing Queries

[Query statements](parser/Query.md) are handled by [QueryExecutor](rest/QueryExecutor.md#handleQuery) (in [REST](rest/index.md) mode).

In the end, [EngineExecutor](EngineExecutor.md) is requested to [plan a query for execution](EngineExecutor.md#planQuery) (that gives an [ExecutionStep](ExecutionStep.md) and an [OutputNode](planner/OutputNode.md)).

## Executing DDL Commands

[DdlCommand](DdlCommand.md)s (e.g. [CREATE STREAM](parser/CreateStream.md)) are planned for execution using [EngineExecutor](EngineExecutor.md#plan) and executed by (per command-line options):

* [DistributingExecutor](rest/DistributingExecutor.md#execute)
* [StatementExecutor](rest/StatementExecutor.md#handleExecutableDdl)

`DistributingExecutor` uses a transactional Kafka producer to [enqueue the command](rest/CommandQueue.md#enqueueCommand) (to the [CommandQueue](#commandQueue)) that is then fetched by [CommandRunner](rest/CommandRunner.md#fetchAndRunCommands).

[CommandRunner](rest/CommandRunner.md) uses [InteractiveStatementExecutor](rest/InteractiveStatementExecutor.md) to [execute commands](rest/CommandRunner.md#executeStatement).

When requested to [handle a statement](rest/InteractiveStatementExecutor.md#handleStatement) (as part of an enqueued command), `InteractiveStatementExecutor` uses [KsqlEngine](KsqlEngine.md) to [execute a ksql plan](KsqlEngine.md#execute).

For [DdlCommand](DdlCommand.md)s, [EngineExecutor](EngineExecutor.md) uses [EngineContext](EngineContext.md) to [execute it](EngineContext.md#executeDdl) (using [DdlCommandExec](DdlCommandExec.md#execute)).

## Kafka Streams

ksqlDB uses [Kafka Streams]({{ book.kafka_streams }}) to [build a physical query plan](QueryEngine.md#buildPhysicalPlan) and for [query implementation](QueryBuilder.md#buildQueryImplementation).

[LogicalPlanner](planner/LogicalPlanner.md#buildPersistentLogicalPlan) plans [Query](parser/Query.md) statements (after analysis) into an [OutputNode](planner/OutputNode.md) (that can [build a SchemaKStream](planner/PlanNode.md#buildStream) that in turn can be requested for an [ExecutionStep](SchemaKStream.md#getSourceStep)).

## Vert.x

[ksqlDB API server](api/Server.md) uses [Vert.x](https://vertx.io/) for HTTP communication using [ServerVerticle](api/ServerVerticle.md).

## Libraries Used I Found Interesting

* [ClassGraph](https://github.com/classgraph/classgraph) to [load UDFs](functions/UserFunctionLoader.md#loadFunctions)

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
