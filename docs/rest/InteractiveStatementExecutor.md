# InteractiveStatementExecutor

## <span id="handleStatement"> handleStatement

```java
void handleStatement(
  QueuedCommand queuedCommand)
```

`handleStatement`...FIXME

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

If the given [Command](Command.md) has a [KsqlPlan](Command.md#getPlan), `handleStatementWithTerminatedQueries` [executes the plan](#executePlan).

Otherwise, `handleStatementWithTerminatedQueries`...FIXME

`handleStatementWithTerminatedQueries` is used when:

* `InteractiveStatementExecutor` is requested to [handleStatement](#handleStatement) and [handleRestore](#handleRestore)

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

`executePlan` requests the [KsqlEngine](#ksqlEngine) to [execute the plan](../KsqlEngine.md#execute).
