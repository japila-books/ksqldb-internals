# ListSourceExecutor

`ListSourceExecutor` is a [CustomExecutors](CustomExecutors.md) to handle the following statements:

* [LIST STREAMS](#streams)
* [DESCRIBE SOURCE](#columns)
* _others_

## <span id="streams"> Handling LIST STREAMS Statement

```java
StatementExecutorResponse streams(
  ConfiguredStatement<ListStreams> statement,
  SessionProperties sessionProperties,
  KsqlExecutionContext executionContext,
  ServiceContext serviceContext)
```

`streams` [getSpecificStreams](#getSpecificStreams).

`streams`...FIXME

---

`streams` is used when:

* `CustomExecutors` is requested to [handle LIST STREAMS statement](CustomExecutors.md#LIST_STREAMS)

## <span id="columns"> Handling DESCRIBE SOURCE Statement

```java
StatementExecutorResponse columns(
  ConfiguredStatement<ShowColumns> statement,
  SessionProperties sessionProperties,
  KsqlExecutionContext executionContext,
  ServiceContext serviceContext)
```

`columns` [describes the DataSource](#describeSource) (of the given [ShowColumns](../parser/ShowColumns.md#table) statement).

---

`columns` is used when:

* `CustomExecutors` is requested to [handle DESCRIBE SOURCE statement](CustomExecutors.md#SHOW_COLUMNS)
* `CustomValidators` is requested to [validate DESCRIBE SOURCE statement](CustomValidators.md#SHOW_COLUMNS)

## <span id="describeSource"> describeSource

```java
SourceDescriptionWithWarnings describeSource(
  KsqlConfig ksqlConfig,
  KsqlExecutionContext ksqlExecutionContext,
  ServiceContext serviceContext,
  SourceName name,
  boolean extended,
  ConfiguredStatement<? extends StatementWithExtendedClause> statement,
  SessionProperties sessionProperties,
  Collection<SourceDescription> remoteSourceDescriptions)
```

`describeSource` looks up the [DataSource](../DataSource.md) (by the given `name`) in the [MetaStore](../KsqlExecutionContext.md#getMetaStore) (of the given [KsqlExecutionContext](../KsqlExecutionContext.md)).

`describeSource` [finds all the running (persistent) queries](#getQueries) (in the given [KsqlExecutionContext](../KsqlExecutionContext.md)). The queries include reading and writing queries (by [getSourceNames](../PersistentQueryMetadata.md#getSourceNames) and [getSinkName](../PersistentQueryMetadata.md#getSinkName), respectively).

`describeSource` [collects DROP constraints](#getSourceConstraints) of the `name` source (if there are any).

With `extended` flag enabled, `describeSource` [queryOffsetSummaries](#queryOffsetSummaries) (for writing queries only).

`describeSource` creates a `SourceDescription` with the [metricCollectors](../KsqlExecutionContext.md#metricCollectors) of the given [KsqlExecutionContext](../KsqlExecutionContext.md) (among the other metadata).

---

`describeSource` is used when:

* `ListSourceExecutor` is requested to [sourceDescriptionList](#sourceDescriptionList) and [columns](#columns)
