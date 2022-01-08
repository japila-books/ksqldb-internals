# InteractiveStatementExecutor

## <span id="handleStatement"> handleStatement

```java
void handleStatement(
  QueuedCommand queuedCommand)
```

`handleStatement` [handles](#handleStatementWithTerminatedQueries) the [Command](Command.md) (from the given `QueuedCommand` and with the `EXECUTE` mode).

`handleStatement` is used when:

* `CommandRunner` is requested to [execute a statement](CommandRunner.md#executeStatement)

## <span id="handleRestore"> handleRestore

```java
void handleRestore(
  QueuedCommand queuedCommand)
```

`handleRestore`...FIXME

`handleRestore` is used when:

* `CommandRunner` is requested to [processPriorCommands](CommandRunner.md#processPriorCommands)

## <span id="handleStatementWithTerminatedQueries"> handleStatementWithTerminatedQueries

```java
void handleStatementWithTerminatedQueries(
  Command command,
  CommandId commandId,
  Optional<CommandStatusFuture> commandStatusFuture,
  Mode mode,
  long offset)
```

`handleStatementWithTerminatedQueries` is used when:

* `InteractiveStatementExecutor` is requested to [handleStatement](#handleStatement) (with `EXECUTE` mode) and [handleRestore](#handleRestore) (with `RESTORE` mode)

### <span id="handleStatementWithTerminatedQueries-executePlan"> Case 1. Command with KsqlPlan

If the given [Command](Command.md) has a [KsqlPlan](Command.md#getPlan), `handleStatementWithTerminatedQueries` [executes](#executePlan) the [KsqlPlan](../KsqlPlan.md).

### <span id="handleStatementWithTerminatedQueries-others"> Others

Otherwise, `handleStatementWithTerminatedQueries`...FIXME

### <span id="executePlan"> Executing Plan

```java
void executePlan(
  Command command,
  CommandId commandId,
  Optional<CommandStatusFuture> commandStatusFuture,
  KsqlPlan plan,
  Mode mode,
  long offset)
```

`executePlan` requests the [KsqlEngine](#ksqlEngine) to [execute](../KsqlEngine.md#execute) the given [KsqlPlan](../KsqlPlan.md).

`executePlan` increments `queryIdGenerator` internal counter.

For the `KsqlPlan` for a `Query` and the mode `EXECUTE`, `executePlan` requests the query to [start](../QueryMetadata.md#start).
