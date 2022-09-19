# KafkaConfigStore

`KafkaConfigStore` is a [ConfigStore](ConfigStore.md) with a [KsqlConfig](#ksqlConfig) with config overrides from a [Kafka topic](#topicName).

## Creating Instance

`KafkaConfigStore` takes the following to be created:

* [Topic Name](#topicName)
* <span id="currentConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="consumer"> `KafkaConsumer<byte[], byte[]>` factory
* <span id="producer"> `KafkaProducer<StringKey, KsqlProperties>` factory

While being created, `KafkaConfigStore` initializes [KsqlConfig](#ksqlConfig).

`KafkaConfigStore` is created when:

* `StandaloneExecutorFactory` is requested to [create a StandaloneExecutor](StandaloneExecutorFactory.md#create)

## <span id="topicName"> Topic Name

`KafkaConfigStore` is given the name of a Kafka topic when [created](#creating-instance).

The topic name is [computed](../rest/ReservedInternalTopics.md#configsTopic) and [created](../KsqlInternalTopicUtils.md#ensureTopic) when `StandaloneExecutorFactory` is requested to [create a StandaloneExecutor](StandaloneExecutorFactory.md#create).

## <span id="ksqlConfig"><span id="getKsqlConfig"> KsqlConfig

`KafkaConfigStore` creates a new [KsqlConfig](../KsqlConfig.md) when [created](#creating-instance).

`KafkaConfigStore` creates a `KafkaWriteOnceStore` to `readMaybeWrite` a `KsqlProperties` from the [Kafka topic](#topicName) (with `KsqlProperties.createFor` with the [currentConfig](#currentConfig)).

!!! fixme "What does this step do exactly?"

In the end, `KafkaConfigStore` requests the [current KsqlConfig](#currentConfig) to [overrideBreakingConfigsWithOriginalValues](../KsqlConfig.md#overrideBreakingConfigsWithOriginalValues) with the `KsqlProperties`.

### getKsqlConfig

```java
KsqlConfig getKsqlConfig()
```

`getKsqlConfig` returns the [KsqlConfig](#ksqlConfig).

---

`getKsqlConfig` is used when:

* `StandaloneExecutorFactory` is requested to [create a StandaloneExecutor](StandaloneExecutorFactory.md#create)
