# QueryApplicationId

## <span id="build"> build

```java
String build(
  KsqlConfig config,
  boolean persistent,
  QueryId queryId)
```

`build`...FIXME

`build` is used when:

* `QueryBuilder` is requested to `buildTransientQuery` and `buildPersistentQueryInDedicatedRuntime`
* `ListSourceExecutor` is requested to `queryOffsetSummaries`
* `KsqlRestoreCommandTopic` is requested to `maybeCleanUpQuery`
