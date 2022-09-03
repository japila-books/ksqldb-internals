# Executor

`Executor` is an [abstraction](#contract) of [executors](#implementations) that can [execute DDL commands](#execute).

## Contract

### <span id="executeAlterSource"> executeAlterSource

```java
DdlCommandResult executeAlterSource(
  AlterSourceCommand alterSource)
```

Used when:

* `AlterSourceCommand` is requested to [execute](AlterSourceCommand.md#execute)

### <span id="executeCreateStream"> executeCreateStream

```java
DdlCommandResult executeCreateStream(
  CreateStreamCommand createStreamCommand)
```

Used when:

* `CreateStreamCommand` is requested to [execute](CreateStreamCommand.md#execute)

### <span id="executeCreateTable"> executeCreateTable

```java
DdlCommandResult executeCreateTable(
  CreateTableCommand createTableCommand);
```

Used when:

* `CreateTableCommand` is requested to [execute](CreateTableCommand.md#execute)

### <span id="executeDropSource"> executeDropSource

```java
DdlCommandResult executeDropSource(
  DropSourceCommand dropSource)
```

Used when:

* `DropSourceCommand` is requested to [execute](DropSourceCommand.md#execute)

### <span id="executeDropType"> executeDropType

```java
DdlCommandResult executeDropType(
  DropTypeCommand dropType)
```

Used when:

* `DropTypeCommand` is requested to [execute](DropTypeCommand.md#execute)

### <span id="executeRegisterType"> executeRegisterType

```java
DdlCommandResult executeRegisterType(
  RegisterTypeCommand registerType)
```

Used when:

* `RegisterTypeCommand` is requested to [execute](RegisterTypeCommand.md#execute)

## Implementations

* [DdlCommandExec.Executor](DdlCommandExec.Executor.md)

## <span id="execute"> Executing DDL Command

```java
DdlCommandResult execute(
  DdlCommand command)
```

`execute` requests the given [DdlCommand](DdlCommand.md) to [execute](DdlCommand.md#execute) (with this `Executor`).

---

`execute` is used when:

* `DdlCommandExec` is requested to [execute a DDL command](DdlCommandExec.md#execute)
