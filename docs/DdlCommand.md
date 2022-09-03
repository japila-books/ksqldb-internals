# DdlCommand

`DdlCommand` is an [abstraction](#contract) of [DDL commands](#implementations) that can be [executed](#execute) (by [Executor](Executor.md#execute)).

## Contract

### <span id="execute"> Executing DDL Command

```java
DdlCommandResult execute(
  Executor executor)
```

Used when:

* `Executor` is reuqested to [execute a DDL command](Executor.md#execute)

## Implementations

* [AlterSourceCommand](AlterSourceCommand.md)
* [CreateSourceCommand](CreateSourceCommand.md)
* [DropSourceCommand](DropSourceCommand.md)
* [DropTypeCommand](DropTypeCommand.md)
* [RegisterTypeCommand](RegisterTypeCommand.md)
