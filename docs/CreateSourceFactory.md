# CreateSourceFactory

## Creating Instance

`CreateSourceFactory` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="metaStore"> [MetaStore](MetaStore.md)

`CreateSourceFactory` is created alongside a [CommandFactories](CommandFactories.md#createSourceFactory).

## <span id="createStreamCommand"> Creating CreateStreamCommand

```java
CreateStreamCommand createStreamCommand(
  CreateStream statement,
  KsqlConfig ksqlConfig)
```

`createStreamCommand` requests the [MetaStore](#metaStore) for the [DataSource](MetaStore.md#getSource) for the given [CreateStream](parser/CreateStream.md).

`createStreamCommand` [throwIfCreateOrReplaceOnSourceStreamOrTable](#throwIfCreateOrReplaceOnSourceStreamOrTable).

In the end, `createStreamCommand` creates a [CreateStreamCommand](CreateStreamCommand.md).

`createStreamCommand` is used when:

* `CommandFactories` is requested to [handle a CreateStream DDL statement](CommandFactories.md#handleCreateStream)

## <span id="throwIfCreateOrReplaceOnSourceStreamOrTable"> throwIfCreateOrReplaceOnSourceStreamOrTable

```java
void throwIfCreateOrReplaceOnSourceStreamOrTable(
  CreateSource createSource,
  DataSource existingSource)
```

`throwIfCreateOrReplaceOnSourceStreamOrTable` throws a `KsqlException` when the given [CreateSource](parser/CreateSource.md) is as follows:

1. [CREATE OR REPLACE](parser/CreateSource.md#isOrReplace)
1. [SOURCE](parser/CreateSource.md#isSource) or the given [DataSource](DataSource.md) is a [source](DataSource.md#isSource)

```text
Cannot add [stream|table] '[source-name]':
CREATE OR REPLACE is not supported on source [stream|table]s.
```

`throwIfCreateOrReplaceOnSourceStreamOrTable` is used when:

* `CreateSourceFactory` is requested for [CreateStreamCommand](#createStreamCommand) and [CreateTableCommand](#createTableCommand) DDL commands

## <span id="buildFormats"> buildFormats

```java
Formats buildFormats(
  SourceName name,
  LogicalSchema schema,
  CreateSourceProperties props,
  KsqlConfig ksqlConfig)
```

`buildFormats`...FIXME

`buildFormats` is used when:

* `CreateSourceFactory` is requested for a [CreateStreamCommand](CreateSourceFactory.md#createStreamCommand) and [CreateTableCommand](CreateSourceFactory.md#createTableCommand)
