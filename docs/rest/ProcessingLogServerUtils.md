# ProcessingLogServerUtils

## <span id="maybeCreateProcessingLogTopic"> maybeCreateProcessingLogTopic

```java
Optional<String> maybeCreateProcessingLogTopic(
  KafkaTopicClient topicClient,
  ProcessingLogConfig config,
  KsqlConfig ksqlConfig)
```

`maybeCreateProcessingLogTopic`...FIXME

---

`maybeCreateProcessingLogTopic` is used when:

* `KsqlRestApplication` is requested to [initialize](KsqlRestApplication.md#initialize)
* `StandaloneExecutor` is requested to [startAsync](../headless/StandaloneExecutor.md#startAsync)
