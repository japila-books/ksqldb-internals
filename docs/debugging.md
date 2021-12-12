# Debugging ksqlDB

Start [KsqlServerMain](rest/KsqlServerMain.md) with the following JPDA configuration as part of `KSQL_OPTS` environment variable:

```text
KSQL_OPTS="-agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=*:5005" \
./bin/ksql-run-class io.confluent.ksql.rest.server.KsqlServerMain \
  config/ksql-server.properties \
  --queries-file create-stream.sql
```

Create a new Configuration for Remove JVM Debug to attach to `KsqlServerMain`. Enjoy!
