# KsqlStream

`KsqlStream` is a [StructuredDataSource](StructuredDataSource.md) with [KSTREAM](DataSource.md#KSTREAM) type.

## Creating Instance

`KsqlStream` takes the following to be created:

* <span id="sqlExpression"> SQL expression
* <span id="datasourceName"> Source name
* <span id="schema"> `LogicalSchema`
* <span id="timestampExtractionPolicy"> `TimestampColumn`
* <span id="isKsqlSink"> `isKsqlSink` flag
* <span id="ksqlTopic"> `KsqlTopic`
* <span id="isSourceStream"> `isSourceStream` flag

`KsqlStream` is created when:

* `DdlCommandExec.Executor` is requested to [execute CreateStreamCommand](DdlCommandExec.Executor.md#executeCreateStream)
* `KsqlStream` is requested to [with](#with)

## <span id="with"> with

```java
DataSource with(
  String sql,
  LogicalSchema schema)
```

`with` is part of the [DataSource](DataSource.md#with) abstraction.

---

`with` creates a new copy of this `KsqlStream` with the following changes:

* [SQL expression](#sqlExpression) is replaced with the [current SQL expression](#getSqlExpression) with the given `sql` appended
* [LogicalSchema](#schema) replaced with the given one
