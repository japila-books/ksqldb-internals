# KsqlInternalTopicUtils

## <span id="ensureTopic"> ensureTopic

```java
void ensureTopic(
  String name,
  KsqlConfig ksqlConfig,
  KafkaTopicClient topicClient)
```

`ensureTopic`...FIXME

---

`ensureTopic` is used when:

* `KsqlRestApplication` is requested to [registerCommandTopic](rest/KsqlRestApplication.md#registerCommandTopic)
* `StandaloneExecutorFactory` is requested to [create a StandaloneExecutor](rest/StandaloneExecutorFactory.md#create)
* `KsqlRestoreCommandTopic` is requested to `restore`
