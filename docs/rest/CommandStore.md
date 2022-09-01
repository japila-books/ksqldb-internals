# CommandStore

`CommandStore` is a [CommandQueue](CommandQueue.md) that uses the [CommandTopic](#commandTopic) for [fetching commands](#getNewCommands).

`CommandStore` is used to create the following services while [building a KsqlRestApplication](KsqlRestApplication.md#buildApplication):

* [StreamedQueryResource](StreamedQueryResource.md#commandQueue)
* [CommandRunner](CommandRunner.md#commandStore)
* [KsqlRestApplication](KsqlRestApplication.md#commandStore)

<figure markdown>
  ![CommandStore](../images/CommandStore.png)
</figure>

## Creating Instance

`CommandStore` takes the following to be created:

* <span id="commandTopicName"> Name of the Command Topic
* [CommandTopic](#commandTopic)
* <span id="sequenceNumberFutureStore"> `SequenceNumberFutureStore`
* <span id="kafkaConsumerProperties"> Kafka Consumer Properties
* <span id="kafkaProducerProperties"> Kafka Producer Properties
* <span id="commandQueueCatchupTimeout"> Command Queue Catchup Timeout
* <span id="commandIdSerializer"> `Serializer<CommandId>`
* <span id="commandSerializer"> `Serializer<Command>`
* <span id="commandIdDeserializer"> `Deserializer<CommandId>`
* <span id="commandTopicBackup"> `CommandTopicBackup`

`CommandStore` is created using [Factory.create](#create) utility.

### <span id="commandTopic"> CommandTopic

`CommandStore` is given a [CommandTopic](CommandTopic.md) when [created](#creating-instance).

The `CommandTopic` is [started](CommandTopic.md#start) in [start](#start) and runs until [close](#close).

Used when:

* [getNewCommands](#getNewCommands)
* [getCommandTopicName](#getCommandTopicName)
* [getRestoreCommands](#getRestoreCommands)
* [isEmpty](#isEmpty)
* [completeSatisfiedSequenceNumberFutures](#completeSatisfiedSequenceNumberFutures)
* [wakeup](#wakeup)

## <span id="create"><span id="Factory"> Creating CommandStore

```java
CommandStore create(
  KsqlConfig ksqlConfig,
  String commandTopicName,
  Duration commandQueueCatchupTimeout,
  Map<String, Object> kafkaConsumerProperties,
  Map<String, Object> kafkaProducerProperties,
  KafkaTopicClient internalTopicClient)
```

`create` adds the following configuration properties to the given `kafkaConsumerProperties` (possibly overriding the current value if set).

Configuration Property | Value
-----------------------|---------
 `isolation.level` | `READ_COMMITTED`
 `auto.offset.reset` | `none`

`create` adds the following configuration properties to the given `kafkaProducerProperties` (possibly overriding the current value if set).

Configuration Property | Value
-----------------------|---------
 `transactional.id` | [ksql.service.id](../KsqlConfig.md#KSQL_SERVICE_ID_CONFIG)
 `acks` | `all`

`create` creates a `CommandTopicBackup` (based on [ksql.metastore.backup.location](../KsqlConfig.md#KSQL_METASTORE_BACKUP_LOCATION)).

In the end, `create` creates a [CommandStore](#creating-instance) with the following:

* The given `commandTopicName`
* A new [CommandTopic](CommandTopic.md)
* A new `SequenceNumberFutureStore`
* _others_

---

`create` is used when:

* `KsqlRestApplication` utility is used to [build a KsqlRestApplication instance](KsqlRestApplication.md#buildApplication)

## <span id="start"> Starting Up

```java
void start()
```

`start` requests the [CommandTopic](#commandTopic) to [start](CommandTopic.md#start).

---

`start` is used when:

* `KsqlRestApplication` is requested to [initialize](KsqlRestApplication.md#initialize)

## <span id="enqueueCommand"> enqueueCommand

```java
QueuedCommandStatus enqueueCommand(
  CommandId commandId,
  Command command,
  Producer<CommandId, Command> transactionalProducer)
```

`enqueueCommand` is part of the [CommandQueue](CommandQueue.md#enqueueCommand) abstraction.

---

`enqueueCommand` creates a `ProducerRecord` ([Apache Kafka]({{ book.kafka }}/clients/producer/ProducerRecord)) as follows:

* Topic: [commandTopicName](#commandTopicName)
* Partition: `0`
* Key: the given `commandId`
* Value: the given [Command](Command.md)

`enqueueCommand` requests the given `transactionalProducer` to send the record.

`enqueueCommand` returns a `QueuedCommandStatus` with the record offset (and a `CommandStatusFuture`).

## <span id="getNewCommands"> Fetching New Commands

```java
List<QueuedCommand> getNewCommands(
  Duration timeout)
```

`getNewCommands` is part of the [CommandQueue](CommandQueue.md#getNewCommands) abstraction.

---

`getNewCommands` requests the [CommandTopic](#commandTopic) for [new commands](CommandTopic.md#getNewCommands) (`ConsumerRecord<byte[], byte[]>`s).

`getNewCommands` creates a `QueuedCommand` for every new command with a non-`null` value.

`getNewCommands` returns the `QueuedCommand`s.

## Logging

Enable `ALL` logging level for `io.confluent.ksql.rest.server.computation.CommandStore` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.io.confluent.ksql.rest.server.computation.CommandStore=ALL
```

Refer to [Logging](../logging.md).
