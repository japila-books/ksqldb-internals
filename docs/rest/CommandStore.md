# CommandStore

## Creating Instance

`CommandStore` takes the following to be created:

* <span id="commandTopicName"> Name of the Command Topic
* <span id="commandTopic"> `CommandTopic`
* <span id="sequenceNumberFutureStore"> `SequenceNumberFutureStore`
* <span id="kafkaConsumerProperties"> Kafka Consumer Properties
* <span id="kafkaProducerProperties"> Kafka Producer Properties
* <span id="commandQueueCatchupTimeout"> Command Queue Catchup Timeout
* <span id="commandIdSerializer"> `Serializer<CommandId>`
* <span id="commandSerializer"> `Serializer<Command>`
* <span id="commandIdDeserializer"> `Deserializer<CommandId>`
* <span id="commandTopicBackup"> `CommandTopicBackup`

`CommandStore` is created using [create](#create) utility.

### <span id="create"> Creating CommandStore

```java
CommandStore create(
  KsqlConfig ksqlConfig,
  String commandTopicName,
  Duration commandQueueCatchupTimeout,
  Map<String, Object> kafkaConsumerProperties,
  Map<String, Object> kafkaProducerProperties,
  ServiceContext serviceContext)
```

`create`...FIXME

`create` is used when:

* `KsqlRestApplication` utility is used to [build a KsqlRestApplication instance](KsqlRestApplication.md#buildApplication)

## <span id="enqueueCommand"> enqueueCommand

```java
QueuedCommandStatus enqueueCommand(
  CommandId commandId,
  Command command,
  Producer<CommandId, Command> transactionalProducer)
```

`enqueueCommand` creates a `ProducerRecord` ([Apache Kafka]({{ book.kafka }}/clients/producer/ProducerRecord)) as follows:

* Topic: [commandTopicName](#commandTopicName)
* Partition: `0`
* Key: the given `commandId`
* Value: the given [Command](Command.md)

`enqueueCommand` requests the given `transactionalProducer` to send the record.

`enqueueCommand` returns a `QueuedCommandStatus` with the record offset (and a `CommandStatusFuture`).

`enqueueCommand` is part of the [CommandQueue](CommandQueue.md#enqueueCommand) abstraction.
