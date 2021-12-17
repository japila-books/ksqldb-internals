# ReservedInternalTopics

## Creating Instance

`ReservedInternalTopics` takes the following to be created:

* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)

`ReservedInternalTopics` is created mainly alongside a [KsqlServerEndpoints](KsqlServerEndpoints.md#reservedInternalTopics) and a [DistributingExecutor](DistributingExecutor.md#internalTopics) but also when:

* `InsertValuesExecutor` is requested to `getDataSource`
* `ListTopicsExecutor` is requested to `listTopics`

## Internal KSQL Command Topic

### <span id="commandTopic"> commandTopic

```java
String commandTopic(
  KsqlConfig ksqlConfig)
```

`commandTopic`...FIXME

`commandTopic` is used when:

* `KafkaBrokerCheck` is requested to `check`
* `KsqlRestApplication` utility is used to [build a KsqlRestApplication](KsqlRestApplication.md#buildApplication) (and creates a [CommandStore](CommandStore.md#create))
* `KsqlRestoreCommandTopic` is created

### <span id="KSQL_COMMAND_TOPIC_SUFFIX"> Topic Name Prefix

`ReservedInternalTopics` assumes `command_topic` as the prefix of the name of the [command topic](#commandTopic).
