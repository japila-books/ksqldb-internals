# CommandFactories

`CommandFactories` is a [DdlCommandFactory](DdlCommandFactory.md).

## Creating Instance

`CommandFactories` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="metaStore"> [MetaStore](MetaStore.md)

`CommandFactories` is created alongside an [EngineContext](EngineContext.md#ddlCommandFactory)

## <span id="FACTORIES"> Command Factories

`CommandFactories` creates `FACTORIES` collection of handlers (_functions_) for [DdlStatement](parser/DdlStatement.md)s to produce `DdlCommand`s.

DdlStatement    | Handler
----------------|---------------------
 [CreateStream](parser/CreateStream.md) | [handleCreateStream](#handleCreateStream)
 [CreateTable](parser/CreateTable.md)   | `handleCreateTable`
 `DropStream`   | `handleDropStream`
 `DropTable`    | `handleDropTable`
 `RegisterType` | `handleRegisterType`
 `DropType`     | `handleDropType`
 `AlterSource`  | `handleAlterSource`

The `FACTORIES` is used in [create](#create).

## <span id="create"> Creating DdlCommand

`create` is part of the [DdlCommandFactory](DdlCommandFactory.md#create) abstraction.

### <span id="create-DdlStatement"> DdlStatement

```java
DdlCommand create(
  String sqlExpression,
  DdlStatement ddlStatement,
  SessionConfig config)
```

`create` looks up (the class of) the given [DdlStatement](parser/DdlStatement.md) in the [FACTORIES](#FACTORIES) registry and requests it to handle it (and produce a `DdlCommand`).

Unless found, `create` throws a `KsqlException`:

```text
Unable to find ddl command factory for statement: [class] valid statements:[FACTORIES]
```

### <span id="create-KsqlStructuredDataOutputNode"> KsqlStructuredDataOutputNode

```java
DdlCommand create(
  KsqlStructuredDataOutputNode outputNode)
```

For a `KSTREAM` node output type (of the given `KsqlStructuredDataOutputNode`), `create` requests the [CreateSourceFactory](#createSourceFactory) for a [CreateStreamCommand](CreateSourceFactory.md#createStreamCommand).

Otherwise, `create` requests the [CreateSourceFactory](#createSourceFactory) for a [CreateTableCommand](CreateSourceFactory.md#createTableCommand).

## <span id="handleCreateStream"> handleCreateStream

```java
CreateStreamCommand handleCreateStream(
  CallInfo callInfo,
  CreateStream statement)
```

`handleCreateStream` requests the [CreateSourceFactory](#createSourceFactory) for a [CreateStreamCommand](CreateSourceFactory.md#createStreamCommand) (for the given [CreateStream](parser/CreateStream.md) statement).
