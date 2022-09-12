# DdlCommandExec.Executor

`DdlCommandExec.Executor` is an [Executor](Executor.md).

`DdlCommandExec.Executor` is a private (_internal_) class of [DdlCommandExec](DdlCommandExec.md).

## Creating Instance

`DdlCommandExec.Executor` takes the following to be created:

* <span id="sql"> SQL statement
* <span id="withQuery"> `withQuery` flag
* <span id="withQuerySources"> Query Sources
* <span id="restoreInProgress"> `restoreInProgress` flag

`DdlCommandExec.Executor` is created when:

* `DdlCommandExec` is requested to [execute a command](DdlCommandExec.md#execute)

## <span id="executeAlterSource"> Executing AlterSourceCommand

```java
DdlCommandResult executeAlterSource(
  AlterSourceCommand alterSource)
```

`executeAlterSource` is part of the [Executor](Executor.md#executeAlterSource) abstraction.

---

`executeAlterSource` uses the [name](AlterSourceCommand.md#getSourceName) of the given [AlterSourceCommand](AlterSourceCommand.md) to look up the [DataSource](DataSource.md) in the [MetaStore](DdlCommandExec.md#metaStore).

`executeAlterSource` adds new columns to the [LogicalSchema](DataSource.md#getSchema) of the data source.

`executeAlterSource` requests the [MetaStore](DdlCommandExec.md#metaStore) to [register the altered data source](MutableMetaStore.md#putSource).

### <span id="executeAlterSource-KsqlException"> KsqlExceptions

`executeAlterSource` throws a `KsqlException` if the data source could not be found:

```text
Source [name] does not exist.
```

---

`executeAlterSource` throws a `KsqlException` when the [data source types](DataSource.md#DataSourceType) do not match:

```text
Incompatible data source type is [type], but statement was ALTER [type]
```

---

`executeAlterSource` throws a `KsqlException` when the data source [isCasTarget](DataSource.md#isCasTarget):

```text
ALTER command is not supported for CREATE ... AS statements.
```

---

`executeAlterSource` throws a `KsqlException` if the name of a new column already exists:

```text
Cannot add column [name] to schema. A column with the same name already exists.
```

## <span id="executeCreateStream"> Executing CreateStreamCommand

```java
DdlCommandResult executeCreateStream(
  CreateStreamCommand createStream)
```

`executeCreateStream` is part of the [Executor](Executor.md#executeCreateStream) abstraction.

---

`executeCreateStream` uses the [name](CreateSourceCommand.md#getSourceName) of the given [CreateStreamCommand](CreateStreamCommand.md) to look up the [DataSource](DataSource.md) in the [MetaStore](DdlCommandExec.md#metaStore).

`executeCreateStream` creates a [KsqlStream](KsqlStream.md).

`executeCreateStream` [registers the KsqlStream](MutableMetaStore.md#putSource) (as a new data source) in the [MetaStore](DdlCommandExec.md#metaStore).

`executeCreateStream` [addSourceReferences](MutableMetaStore.md#addSourceReferences) for the `KsqlStream` with the [withQuerySources](#withQuerySources) in the [MetaStore](DdlCommandExec.md#metaStore).
