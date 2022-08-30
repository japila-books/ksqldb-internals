# InteractiveStatementExecutor

## <span id="handleStatement"> Executing Queued Command

```java
void handleStatement(
  QueuedCommand queuedCommand)
```

`handleStatement` [handles](#handleStatementWithTerminatedQueries) the [Command](Command.md) (from the given `QueuedCommand` with `EXECUTE` mode).

---

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

## <span id="handleStatementWithTerminatedQueries"> Executing Command

```java
void handleStatementWithTerminatedQueries(
  Command command,
  CommandId commandId,
  Optional<CommandStatusFuture> commandStatusFuture,
  Mode mode,
  long offset)
```

`handleStatementWithTerminatedQueries` handles the given [Command](Command.md) based on whether it has a [KsqlPlan](#handleStatementWithTerminatedQueries-executePlan) or just a [statement](#handleStatementWithTerminatedQueries-executeStatement).

---

`handleStatementWithTerminatedQueries` is used when:

* `InteractiveStatementExecutor` is requested to [handleStatement](#handleStatement) (with `EXECUTE` mode) and [handleRestore](#handleRestore) (with `RESTORE` mode)

### <span id="handleStatementWithTerminatedQueries-executePlan"> KsqlPlan

If the given [Command](Command.md) has a [KsqlPlan](Command.md#getPlan), `handleStatementWithTerminatedQueries` [executes](#executePlan) the [KsqlPlan](../KsqlPlan.md).

### <span id="handleStatementWithTerminatedQueries-executeStatement"> Statement

`handleStatementWithTerminatedQueries` [puts PARSING status](#putStatus).

`handleStatementWithTerminatedQueries` requests the [StatementParser](#statementParser) to [parse the statement](StatementParser.md#parseSingleStatement) (from the command).

`handleStatementWithTerminatedQueries` [puts EXECUTING status](#putStatus).

`handleStatementWithTerminatedQueries` [executeStatement](#executeStatement).

## <span id="executePlan"> Executing KsqlPlan

```java
void executePlan(
  Command command,
  CommandId commandId,
  Optional<CommandStatusFuture> commandStatusFuture,
  KsqlPlan plan,
  Mode mode,
  long offset,
  boolean restoreInProgress)
```

`executePlan` [builds merged config](#buildMergedConfig).

`executePlan` [puts EXECUTING status](#putStatus) for the command with the message:

```text
Executing statement
```

`executePlan` requests the [KsqlEngine](#ksqlEngine) to [execute](../KsqlEngine.md#execute) the given [KsqlPlan](../KsqlPlan.md).

`executePlan` increments `queryIdGenerator` internal counter.

If this is a [QueryMetadata](../QueryMetadata.md) and the given mode is `EXECUTE`, `executePlan` requests the query to [start](../QueryMetadata.md#start).

In the end, `executePlan` [puts SUCCESS final status](#putFinalStatus) with the command result or the following message:

```text
Created query with ID [queryId]
```

## <span id="executeStatement"> Executing Statement

```java
void executeStatement(
  PreparedStatement<?> statement,
  CommandId commandId,
  Optional<CommandStatusFuture> commandStatusFuture)
```

`executeStatement` branches off based on the type of the statement.

### <span id="executeStatement-TerminateQuery"> TerminateQuery

For [TerminateQuery](../parser/TerminateQuery.md), `executeStatement` [terminateQuery](#terminateQuery) and [puts SUCCESS final status](#putFinalStatus) with the following message:

```text
Query terminated.
```

### <span id="executeStatement-ExecutableDdlStatement"> ExecutableDdlStatement

For [ExecutableDdlStatement](../parser/ExecutableDdlStatement.md), `executeStatement` [throwUnsupportedStatementError](#throwUnsupportedStatementError).

### <span id="executeStatement-CreateAsSelect"> CreateAsSelect

For [CreateAsSelect](../parser/CreateAsSelect.md), `executeStatement` [throwUnsupportedStatementError](#throwUnsupportedStatementError).

### <span id="executeStatement-InsertInto"> InsertInto

For [InsertInto](../parser/InsertInto.md), `executeStatement` [throwUnsupportedStatementError](#throwUnsupportedStatementError).

### <span id="executeStatement-AlterSystemProperty"> AlterSystemProperty

For [AlterSystemProperty](../parser/AlterSystemProperty.md), `executeStatement` requests the [KsqlExecutionContext](#ksqlEngine) to [alterSystemProperty](../KsqlExecutionContext.md#alterSystemProperty) and then to [updateStreamsPropertiesAndRestartRuntime](../KsqlExecutionContext.md#updateStreamsPropertiesAndRestartRuntime).

In the end, `executeStatement` [puts SUCCESS final status](#putFinalStatus) with the following message:

```text
System property [name] was set to [value].
```

### <span id="executeStatement-others"> Other Types

For all other types, `executeStatement` throws a `KsqlException`:

```text
Unexpected statement type: [className]
```

## <span id="putFinalStatus"> putFinalStatus

```java
void putFinalStatus(
  CommandId commandId,
  Optional<CommandStatusFuture> commandStatusFuture,
  CommandStatus status)
```

`putFinalStatus` associates the given `CommandStatus` with the `CommandId` (in [statusStore](#statusStore) registry).

If the given `CommandStatusFuture` is available, `putFinalStatus` sets its final status.

---

`putFinalStatus` is used when:

* `InteractiveStatementExecutor` is requested to [execute a command](#handleStatementWithTerminatedQueries) ([KsqlPlan](#executePlan) or [Statement](#executeStatement))
