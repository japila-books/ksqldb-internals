# Logging

ksqldb uses [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) for logging and uses `etc/ksqldb/log4j.properties` by default.

The default logging level is `INFO` with `stdout` appender.

```text
log4j.rootLogger=INFO, stdout
```

There are various appenders for streams, clients and connect subsystems.
