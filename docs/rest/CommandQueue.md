# CommandQueue

`CommandQueue` is an [abstraction](#contract) of [command queues](#implementations).

## Contract (Subset)

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
