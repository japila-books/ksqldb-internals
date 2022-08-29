# REST

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

[KsqlRestApplication](KsqlRestApplication.md) creates a [CommandRunner](KsqlRestApplication.md#commandRunner) to [process prior commands](CommandRunner.md#processPriorCommands) and then [run new commands](CommandRunner.md#fetchAndRunCommands) continuously (off a [CommandQueue](CommandRunner.md#commandStore)).
