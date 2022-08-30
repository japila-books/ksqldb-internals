# CommandTopic

`CommandTopic` manages a [KafkaConsumer](#commandConsumer) to [fetch new commands](#getNewCommands) (for [CommandStore](CommandStore.md#getNewCommands)) from the [internal command topic](#commandTopicPartition).

## Creating Instance

`CommandTopic` takes the following to be created:

* <span id="commandTopicName"> Name of the internal command topic
* <span id="kafkaConsumerProperties"> `KafkaConsumer` Properties
* <span id="commandTopicBackup"> `CommandTopicBackup`

`CommandTopic` is created when:

* `CommandStore.Factory` is requested to [create a CommandStore](CommandStore.md#create)

### <span id="commandConsumer"> KafkaConsumer

`CommandTopic` can be given a `Consumer<byte[], byte[]>` or [properties](#kafkaConsumerProperties) to create one when [created](#creating-instance).

The `Consumer` is assigned the [commandTopicPartition](#commandTopicPartition) in [start](#start).

The `Consumer` fetches `ConsumerRecord<byte[], byte[]>`s when requested for [new commands](#getNewCommands).

## <span id="commandTopicPartition"> commandTopicPartition

`CommandTopic` creates a Kafka's `TopicPartition` for the [commandTopicName](#commandTopicName) and `0` partition when [created](#creating-instance).

The `commandTopicPartition` is assigned to [commandConsumer](#commandConsumer) in [start](#start).

## <span id="start"> Starting Up

```java
void start()
```

`start` requests the [CommandTopicBackup](#commandTopicBackup) to [initialize](CommandTopicBackup.md#initialize).

`start` requests the [KafkaConsumer](#commandConsumer) to consume records from the [commandTopicPartition](#commandTopicPartition) only (using [KafkaConsumer.assign]({{ kafka.api }}/org/apache/kafka/clients/consumer/KafkaConsumer.html#assign(java.util.Collection))).

---

`start` is used when:

* `CommandStore` is requested to [start](CommandStore.md#start)

## <span id="getNewCommands"> Fetching New Commands

```java
Iterable<ConsumerRecord<byte[], byte[]>> getNewCommands(
  Duration timeout)
```

`getNewCommands` requests the [commandConsumer](#commandConsumer) to fetch records (`ConsumerRecord<byte[], byte[]>`s).

`getNewCommands` [makes a backup of every record](#backupRecord).

---

`getNewCommands` is used when:

* `CommandStore` is requested for [new commands](CommandStore.md#getNewCommands)
