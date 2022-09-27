# CreateSourceFactory

## Creating Instance

`CreateSourceFactory` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="keySerdeFeaturesSupplier"> `keySerdeFeaturesSupplier`
* <span id="valueSerdeFeaturesSupplier"> `valueSerdeFeaturesSupplier`
* <span id="keySerdeFactory"> `KeySerdeFactory`
* [ValueSerdeFactory](#valueSerdeFactory)
* <span id="metaStore"> [MetaStore](MetaStore.md)

`CreateSourceFactory` is created alongside a [CommandFactories](CommandFactories.md#createSourceFactory).

### <span id="valueSerdeFactory"> ValueSerdeFactory

`CreateSourceFactory` is given a [ValueSerdeFactory](formats/ValueSerdeFactory.md) when [created](#creating-instance).

The `ValueSerdeFactory` is used in [validateSerdesCanHandleSchemas](#validateSerdesCanHandleSchemas).

## <span id="createStreamCommand"> Creating CreateStreamCommand

`createStreamCommand` can create a [CreateStreamCommand](CreateStreamCommand.md) for a [CreateStream](#createStreamCommand-CreateStream) statement or a [KsqlStructuredDataOutputNode](#createStreamCommand-KsqlStructuredDataOutputNode).

### <span id="createStreamCommand-CreateStream"> CreateStream

```java
CreateStreamCommand createStreamCommand(
  CreateStream statement,
  KsqlConfig ksqlConfig)
```

`createStreamCommand` [ensureTopicExists](#ensureTopicExists), [buildSchema](#buildSchema) and [buildTimestampColumn](#buildTimestampColumn).

`createStreamCommand` requests the [MetaStore](#metaStore) for the [DataSource](MetaStore.md#getSource) for the given [CreateStream](parser/CreateStream.md).

`createStreamCommand` [throwIfCreateOrReplaceOnSourceStreamOrTable](#throwIfCreateOrReplaceOnSourceStreamOrTable).

In the end, `createStreamCommand` creates a [CreateStreamCommand](CreateStreamCommand.md).

---

`createStreamCommand` is used when:

* `CommandFactories` is requested to [handle a CreateStream DDL statement](CommandFactories.md#handleCreateStream)

### <span id="createStreamCommand-KsqlStructuredDataOutputNode"> KsqlStructuredDataOutputNode

```java
CreateStreamCommand createStreamCommand(
  KsqlStructuredDataOutputNode outputNode)
```

`createStreamCommand` creates a [CreateStreamCommand](CreateStreamCommand.md) for the given [KsqlStructuredDataOutputNode](planner/KsqlStructuredDataOutputNode.md) (with the [isSource](CreateStreamCommand.md#isSource) flag disabled).

`createStreamCommand` is used when:

* `CommandFactories` is requested to [create](CommandFactories.md#create)

## <span id="createTableCommand"> Creating CreateTableCommand

```java
CreateTableCommand createTableCommand(
  CreateTable statement,
  KsqlConfig ksqlConfig)
```

`createTableCommand`...FIXME

`createTableCommand` is used when:

* `CommandFactories` is requested to [handle a CreateTable DDL statement](CommandFactories.md#handleCreateTable)

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
