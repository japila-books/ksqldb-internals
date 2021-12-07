# {{ book.title }}

[ksqlDB](https://ksqldb.io/) is _"the database purpose-built for stream processing applications._"

ksqlDB uses [KsqlServerMain](rest/KsqlServerMain.md) to handle SQL queries (from the [command line](rest/ServerOptions.md#queries-file) or sent through a REST endpoint, e.g. using [ksql](cli/Ksql.md)).

```text
$ ./bin/ksql-server-start config/ksql-server.properties

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

Copyright 2017-2021 Confluent Inc.

Server 7.2.0-0 listening on http://0.0.0.0:8088

To access the KSQL CLI, run:
ksql http://0.0.0.0:8088
```

ksqlDB uses [Ksql](cli/Ksql.md) as the command-line interactive environment.

```text
$ ./bin/ksql
ksql>
```
