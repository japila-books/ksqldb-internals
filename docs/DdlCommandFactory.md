# DdlCommandFactory

`DdlCommandFactory` is an [abstraction](#contract) of [DdlCommand factories](#implementations) that can [create DdlCommands](#create) (for [EngineContext](EngineContext.md#createDdlCommand)).

## Contract

### <span id="create"> Creating DdlCommand

```java
DdlCommand create(
  KsqlStructuredDataOutputNode outputNode)
DdlCommand create(
  String sqlExpression,
  DdlStatement ddlStatement,
  SessionConfig config)
```

Creates a [DdlCommand](DdlCommand.md) for a [KsqlStructuredDataOutputNode](planner/KsqlStructuredDataOutputNode.md) or a [ExecutableDdlStatement](parser/ExecutableDdlStatement.md)

Used when:

* `EngineContext` is requested to [create a DdlCommand](EngineContext.md#createDdlCommand)

## Implementations

* [CommandFactories](CommandFactories.md)
