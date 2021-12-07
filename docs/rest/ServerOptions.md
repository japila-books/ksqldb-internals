# ServerOptions

`ServerOptions` is used to handle the command-line options of [KsqlServerMain](KsqlServerMain.md).

`ServerOptions` uses [Airline](https://rvesse.github.io/airline/) annotation-driven Java library for building Command Line Interfaces (CLIs).

## <span id="config-file"><span id="propertiesFile"><span id="getPropertiesFile"> Properties File

(**required**) A file with the configuration properties for the KSQL Server, KSQL, and underlying Kafka Streams instances.

## <span id="QUERIES_FILE_CONFIG"><span id="ksql.queries.file"><span id="getQueriesFile"><span id="queries-file"> Query File

A path to a query file on the local machine.

A query file can be specified using the following (in the order of precedence):

1. `--queries-file` command-line option
1. `ksql.queries.file` Java property
