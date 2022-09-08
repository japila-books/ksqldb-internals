# CommandQueue

`CommandQueue` is an [abstraction](#contract) of [command queues](#implementations) that [DistributingExecutor](DistributingExecutor.md) uses to [execute statements](DistributingExecutor.md#execute).

## Contract (Subset)

### <span id="createTransactionalProducer"> Creating Transactional Kafka Producer

```java
Producer<CommandId, Command> createTransactionalProducer()
```

Creates a `KafkaProducer` ([Apache Kafka]({{ book.kafka }}/clients/producer/KafkaProducer/))

See [CommandStore](CommandStore.md#createTransactionalProducer)

Used when:

* `DistributingExecutor` is requested to [execute a statement](DistributingExecutor.md#execute)

### <span id="ensureConsumedPast"> ensureConsumedPast

```java
void ensureConsumedPast(
  long seqNum,
  Duration timeout)
```

See [CommandStore](CommandStore.md#ensureConsumedPast)

Used when:

* `CommandStore` is requested to [waitForCommandConsumer](CommandStore.md#waitForCommandConsumer)
* `DefaultCommandQueueSync` is requested to `waitFor`
* `CommandStoreUtil` is requested to [waitForCommandSequenceNumber](CommandStoreUtil.md#waitForCommandSequenceNumber)

### <span id="enqueueCommand"> enqueueCommand

```java
QueuedCommandStatus enqueueCommand(
  CommandId commandId,
  Command command,
  Producer<CommandId, Command> transactionalProducer)
```

Sends a command to the [command topic](CommandTopic.md) (using a [transactional KafkaProducer](#createTransactionalProducer)) for execution

See [CommandStore](CommandStore.md#enqueueCommand)

Used when:

* `DistributingExecutor` is requested to [execute a statement](DistributingExecutor.md#execute)

### <span id="getNewCommands"> Fetching New Commands

```java
List<QueuedCommand> getNewCommands(
  Duration timeout)
```

Fetches `QueuedCommand`s (e.g., from a [CommandTopic](CommandTopic.md))

See [CommandStore](CommandStore.md#getNewCommands)

Used when:

* `CommandRunner` is requested to [fetchAndRunCommands](CommandRunner.md#fetchAndRunCommands)

### <span id="waitForCommandConsumer"> waitForCommandConsumer

```java
void waitForCommandConsumer()
```

See [CommandStore](CommandStore.md#waitForCommandConsumer)

Used when:

* `DistributingExecutor` is requested to [execute a statement](DistributingExecutor.md#execute)

## Implementations

* [CommandStore](CommandStore.md)
