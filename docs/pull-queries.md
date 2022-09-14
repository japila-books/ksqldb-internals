# Pull Queries

**Pull Queries** are non-persistent (_transient_) `SELECT` queries with no `EMIT CHANGES` clause (that is part of [push queries](push-queries.md)).

Pull Queries retrieve the latest result from a source (a materialized view, a table, or a stream) instantly and as of "now".

Pull queries are printed out only in the console.

Pull queries follow a traditional request/response model. They retrieve a finite result from the ksqlDB server and terminate (like in traditional databases).

Pull queries use an eventually consistent consistency model.

## Configuration Properties

All pull queries can be disabled (on a specific ksqlDB server) using [ksql.pull.queries.enable](KsqlConfig.md#KSQL_PULL_QUERIES_ENABLE_CONFIG).

Stream pull queries can be disabled using [ksql.query.pull.stream.enabled](KsqlConfig.md#KSQL_QUERY_STREAM_PULL_QUERY_ENABLED).

## Stream Pull Queries

**Stream Pull Queries** (_pull queries over stream_) are pull queries over streams and are executed using [QueryExecutor](rest/QueryExecutor.md#handleStreamPullQuery).

## Table Pull Queries

**Table Pull Queries** (_pull queries over table_) are pull queries over tables and are executed using [QueryExecutor](rest/QueryExecutor.md#handleTablePullQuery).

## Demo

[Demo: Pull Queries](demo/pull-queries.md)

## Learning Resources

* [Pull]({{ ksqldb.docs }}/concepts/queries/#pull)
* [SELECT (Pull Query)]({{ ksqldb.docs }}/developer-guide/ksqldb-reference/select-pull-query/)
