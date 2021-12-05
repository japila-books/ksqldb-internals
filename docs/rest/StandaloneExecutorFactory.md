# StandaloneExecutorFactory

## <span id="create"> Creating StandaloneExecutor

```java
StandaloneExecutor create(
  Map<String, String> properties,
  String queriesFile,
  String installDir)
```

`create`...FIXME

`create` is used when:

* `KsqlServerMain` is requested to [createExecutable](KsqlServerMain.md#createExecutable) (with a queries file)
