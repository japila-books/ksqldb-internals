# Headless Execution Mode

**Headless Execution Mode** (_standalone execution mode_) uses [StandaloneExecutor](StandaloneExecutor.md) to execute KSQL statements from a [query file](#query-file) only.

Headless execution mode uses neither [REST API](../rest/index.md) (for statement submission) nor Kafka Connect (for connectors).

## Query File

A query file can be specified as follows:

* [--queries-file](../rest/KsqlServerMain.md#queries-file) command-line option
* [ksql.queries.file](../rest/ServerOptions.md#ksql.queries.file) configuration property
