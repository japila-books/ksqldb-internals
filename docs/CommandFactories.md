# CommandFactories

`CommandFactories` is a [DdlCommandFactory](DdlCommandFactory.md).

## Creating Instance

`CommandFactories` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="metaStore"> [MetaStore](MetaStore.md)

`CommandFactories` is created alongside [EngineContext](EngineContext.md#ddlCommandFactory)

## <span id="create"> Creating DdlCommand

`create` is part of the [DdlCommandFactory](DdlCommandFactory.md#create) abstraction.

`create` creates a [DdlCommand](DdlCommand.md) for a [DdlStatement](#create-DdlStatement) or a [KsqlStructuredDataOutputNode](#create-KsqlStructuredDataOutputNode).

### <span id="create-DdlStatement"> DdlStatement

```java
DdlCommand create(
  String sqlExpression,
  DdlStatement ddlStatement,
  SessionConfig config)
```

`create` looks up (the class of) the given [DdlStatement](parser/DdlStatement.md) (in [FACTORIES](#FACTORIES)) to handle it and produce a [DdlCommand](DdlCommand.md).

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

## <span id="FACTORIES"> FACTORIES

`CommandFactories` creates `FACTORIES` lookup table (of handlers) to [create DdlCommands from DdlStatements](#create).

DdlStatement    | Handler | DdlCommand
----------------|---------|-----------
 `AlterSource`  | [handleAlterSource](#handleAlterSource) | [AlterSourceCommand](AlterSourceCommand.md)
 [CreateStream](parser/CreateStream.md) | [handleCreateStream](#handleCreateStream) | [CreateStreamCommand](CreateStreamCommand.md)
 [CreateTable](parser/CreateTable.md)   | [handleCreateTable](#handleCreateTable) | [CreateTableCommand](CreateTableCommand.md)
 `DropStream`   | `handleDropStream` |
 `DropTable`    | `handleDropTable` |
 `DropType`     | `handleDropType` | [DropTypeCommand](DropTypeCommand.md)
 `RegisterType` | `handleRegisterType` | [RegisterTypeCommand](RegisterTypeCommand.md)

### <span id="handleAlterSource"> handleAlterSource

```java
AlterSourceCommand handleAlterSource(
  AlterSource statement)
```

`handleAlterSource` requests the [AlterSourceFactory](#alterSourceFactory) for an [AlterSourceCommand](AlterSourceFactory.md#create) (for the given [AlterSource](parser/AlterSource.md) statement).

### <span id="handleCreateStream"> handleCreateStream

```java
CreateStreamCommand handleCreateStream(
  CallInfo callInfo,
  CreateStream statement)
```

`handleCreateStream` requests the [CreateSourceFactory](#createSourceFactory) for a [CreateStreamCommand](CreateSourceFactory.md#createStreamCommand-CreateStream) (for the given [CreateStream](parser/CreateStream.md) statement).

### <span id="handleCreateTable"> handleCreateTable

```java
CreateTableCommand handleCreateTable(
  CallInfo callInfo,
  CreateTable statement)
```

`handleCreateTable` requests the [CreateSourceFactory](#createSourceFactory) for a [CreateTableCommand](CreateSourceFactory.md#createTableCommand) (for the given [CreateTable](parser/CreateTable.md) statement).
