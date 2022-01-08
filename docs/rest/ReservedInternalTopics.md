# ReservedInternalTopics

## Creating Instance

`ReservedInternalTopics` takes the following to be created:

* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)

`ReservedInternalTopics` is created mainly alongside a [KsqlServerEndpoints](KsqlServerEndpoints.md#reservedInternalTopics) and a [DistributingExecutor](DistributingExecutor.md#internalTopics) but also when:

* `InsertValuesExecutor` is requested to `getDataSource`
* `ListTopicsExecutor` is requested to `listTopics`

## Internal ksqlDB Topics

### <span id="commandTopic"> Command Topic

```java
String commandTopic(
  KsqlConfig ksqlConfig)
```

`commandTopic` [builds the name of a ksqlDB internal topic](#toKsqlInternalTopic) with `command_topic` topic suffix:

```text
_confluent-ksql-[ksql.service.id]_command_topic
```

`commandTopic` is used when:

* `HealthCheckAgent.KafkaBrokerCheck` is requested to `check`
* `KsqlRestApplication` utility is used to [build a KsqlRestApplication](KsqlRestApplication.md#buildApplication) (and creates a [CommandStore](CommandStore.md#create))
* `KsqlRestoreCommandTopic` is created

### <span id="configsTopic"> Configs Topic

```java
String configsTopic(
  KsqlConfig ksqlConfig)
```

`configsTopic` [builds the name of a ksqlDB internal topic](#toKsqlInternalTopic) with `configs` topic suffix:

```text
_confluent-ksql-[ksql.service.id]_configs
```

`configsTopic` is used when:

* `StandaloneExecutorFactory` utility is used to [create a StandaloneExecutor](StandaloneExecutorFactory.md#create)

## <span id="toKsqlInternalTopic"> toKsqlInternalTopic

```java
String toKsqlInternalTopic(
  KsqlConfig ksqlConfig,
  String topicSuffix)
```

`toKsqlInternalTopic` builds a name (of a ksqlDB internal topic) in the following format (based on the [ksql.service.id](../KsqlConfig.md#KSQL_SERVICE_ID_CONFIG) in the given [KsqlConfig](../KsqlConfig.md) and the given `topicSuffix`):

```text
_confluent-ksql-[ksql.service.id]_[topicSuffix]
```

`toKsqlInternalTopic` is used when:

* `ReservedInternalTopics` utility is used for [commandTopic](#commandTopic) and [configsTopic](#configsTopic)
