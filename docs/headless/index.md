# Headless Execution Mode

**Headless Execution Mode** (_standalone execution mode_) is when [KsqlServerMain](../rest/KsqlServerMain.md) is [executed](../rest/KsqlServerMain.md#createExecutable) with a [query file](#query-file).

Headless execution mode uses [StandaloneExecutor](StandaloneExecutor.md) to execute KSQL statements (from the [query file](#query-file) only).

Headless execution mode uses neither [REST API](../rest/index.md) (for statement submission) nor Kafka Connect (for connectors).

## Query File

A query file (_queries file_) can be specified as follows:

* [--queries-file](../rest/KsqlServerMain.md#queries-file) command-line option
* [ksql.queries.file](../rest/ServerOptions.md#ksql.queries.file) configuration property
