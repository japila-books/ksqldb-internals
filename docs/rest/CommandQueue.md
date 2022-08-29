# CommandQueue

`CommandQueue` is an [abstraction](#contract) of [command queues](#implementations).

## Contract (Subset)

### <span id="getNewCommands"> getNewCommands

```java
List<QueuedCommand> getNewCommands(
  Duration timeout)
```

Fetches `QueuedCommand`s

Used when:

* `CommandRunner` is requested to [fetchAndRunCommands](CommandRunner.md#fetchAndRunCommands)

### <span id="enqueueCommand"> enqueueCommand

```java
QueuedCommandStatus enqueueCommand(
  CommandId commandId,
  Command command,
  Producer<CommandId, Command> transactionalProducer)
```

See [CommandStore](CommandStore.md#enqueueCommand)

Used when:

* `DistributingExecutor` is requested to [execute a SQL statement](DistributingExecutor.md#execute)

## Implementations

* [CommandStore](CommandStore.md)
