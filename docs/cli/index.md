# ksql CLI

`ksql` uses [Ksql](Ksql.md) to post KSQL statements (over HTTP 2.0) to a [ksqlDB API server](../rest/KsqlRestApplication.md) for execution (on [KsqlEngine](../KsqlEngine.md)).

`ksql` is a REST client that communicates with the [REST URIs](../api/ServerVerticle.md#uris).

```console
$ ./bin/ksql --help
NAME
        ksql - KSQL CLI

SYNOPSIS
        ksql [ --config-file <configFile> ]
                [ --confluent-api-key <ccloudApiKey> ]
                [ --confluent-api-secret <ccloudApiSecret> ]
                [ {--define | -d} <definedVars>... ]
                [ {--execute | -e} <execute> ] [ {--file | -f} <scriptFile> ]
                [ {-h | --help} ] [ --output <outputFormat> ]
                [ {--password | -p} <password> ]
                [ --query-row-limit <streamedQueryRowLimit> ]
                [ --query-timeout <streamedQueryTimeoutMs> ]
                [ {--user | -u} <userName> ] [--] [ <server> ]
...
```

`ksql` supports executing ksql statements using `execute` and `file` options or interactively.

```text
        --execute <execute>, -e <execute>
            Execute one or more SQL statements and quit.

        --file <scriptFile>, -f <scriptFile>
            Execute commands from a file and exit.
```

`ksql` accepts the address of the ksqldb server to connect to or assumes `http://localhost:8088`.

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

```text
ksql> help

Description:
	The KSQL CLI provides a terminal-based interactive shell for running queries. Each command should be on a separate line. For KSQL command syntax, see the documentation at https://docs.ksqldb.io/en/latest/developer-guide/syntax-reference/
...

ksql> version
Version: 0.27.2

ksql> server
http://localhost:8088

ksql> output
Current output format: TABULAR

ksql> exit
Exiting ksqlDB.
```
