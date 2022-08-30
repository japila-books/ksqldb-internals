# CommandStore

`CommandStore` is a [CommandQueue](CommandQueue.md).

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

`CommandStore` is created using [Factory.create](#create) utility.

## <span id="create"><span id="Factory"> Factory.create

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
* A new `CommandTopic`
* A new `SequenceNumberFutureStore`
* _others_

---

`create` is used when:

* `KsqlRestApplication` utility is used to [build a KsqlRestApplication instance](KsqlRestApplication.md#buildApplication)

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
