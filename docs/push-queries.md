# Push Queries

**Push Queries** are non-persistent `SELECT` queries with `EMIT CHANGES` clause.

Push queries are executed continuously and never end unless cancelled by a user or `LIMIT` is reached. They are a perfect fit for real-time updates from a source (a stream or a materialized view).
