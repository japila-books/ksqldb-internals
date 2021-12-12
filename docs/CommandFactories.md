# CommandFactories

`CommandFactories` is a [DdlCommandFactory](DdlCommandFactory.md).

## Creating Instance

`CommandFactories` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="metaStore"> [MetaStore](MetaStore.md)

`CommandFactories` is created alongside a [EngineContext](EngineContext.md#ddlCommandFactory)

## <span id="create"> Creating DdlCommand

`create` is part of the [DdlCommandFactory](DdlCommandFactory.md#create) abstraction.

### <span id="create-KsqlStructuredDataOutputNode"> KsqlStructuredDataOutputNode

```java
DdlCommand create(
  KsqlStructuredDataOutputNode outputNode)
```

For a `KSTREAM` node output type (of the given `KsqlStructuredDataOutputNode`), `create` requests the [CreateSourceFactory](#createSourceFactory) for a [CreateStreamCommand](CreateSourceFactory.md#createStreamCommand).

Otherwise, `create` requests the [CreateSourceFactory](#createSourceFactory) for a [CreateTableCommand](CreateSourceFactory.md#createTableCommand).

### <span id="create-DdlStatement"> DdlStatement

```java
DdlCommand create(
  String sqlExpression,
  DdlStatement ddlStatement,
  SessionConfig config)
```

`create`...FIXME
