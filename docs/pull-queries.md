# Pull Queries

**Pull Queries** are non-persistent `SELECT` queries with no `EMIT CHANGES` clause (that is part of [push queries](push-queries.md)).

Pull Queries retrieve the latest result from a source (a materialized view, a table, or a stream) instantly and as of "now".

Pull queries are printed out only in the console.

Pull queries follow a traditional request/response model. They retrieve a finite result from the ksqlDB server and terminate (like in traditional databases).

Pull queries use an eventually consistent consistency model.

!!! note
    Learn more in the official documentation of ksqlDB [here](https://docs.ksqldb.io/en/latest/developer-guide/ksqldb-reference/select-pull-query/) and [here](https://docs.ksqldb.io/en/latest/concepts/queries/#pull).

## Demo

[Demo: Pull Queries](demo/pull-queries.md)
