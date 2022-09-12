# REST Execution Mode

In **REST execution mode**, ksqlDB uses [KsqlRestApplication](KsqlRestApplication.md) as the ksqlDB REST API server (and expose REST endpoints to HTTP clients, e.g. [ksql](../cli/Ksql.md) shell script).

## ksql-server-start Shell Script

`ksql-server-start` shell script is used to launch [KsqlServerMain](KsqlServerMain.md).

```text
./bin/ksql-run-class io.confluent.ksql.rest.server.KsqlServerMain
```

```console
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
