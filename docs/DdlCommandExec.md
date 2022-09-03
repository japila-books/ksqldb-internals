# DdlCommandExec

## Creating Instance

`DdlCommandExec` takes the following to be created:

* <span id="metaStore"> [MutableMetaStore](MutableMetaStore.md)

`DdlCommandExec` is created along with [EngineContext](EngineContext.md#ddlCommandExec).

## <span id="execute"> Executing DDL Command

```java
DdlCommandResult execute(
  String sql,
  DdlCommand ddlCommand,
  boolean withQuery,
  Set<SourceName> withQuerySources) // (1)!
DdlCommandResult execute(
  String sql,
  DdlCommand ddlCommand,
  boolean withQuery,
  Set<SourceName> withQuerySources,
  boolean restoreInProgress)
```

1. Turns `restoreInProgress` off (`false`)

`execute` creates an [Executor](Executor.md) to [execute](Executor.md#execute) the given [DdlCommand](DdlCommand.md).

---

`execute` is used when:

* `EngineContext` is requested to [execute a DDL command](EngineContext.md#executeDdl)
