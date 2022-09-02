# Debugging ksqlDB

Start [KsqlServerMain](rest/KsqlServerMain.md) (directly or indirectly using [ksql-server-start](rest/index.md) shell script) with the following JPDA configuration as part of `KSQL_OPTS` environment variable.

```text
export KSQL_OPTS="-agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=*:5005"
```

`suspend=y` will suspend the JVM process until you attach to the `address=*:5005`.

```console
$ ./bin/ksql-server-start config/ksql-server.properties
Listening for transport dt_socket at address: 5005
```

Attach to the process (e.g., in IntelliJ IDEA) and step through the code. _Enjoy!_
