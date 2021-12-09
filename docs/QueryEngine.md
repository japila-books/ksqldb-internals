# QueryEngine

## <span id="buildQueryLogicalPlan"> buildQueryLogicalPlan

```java
OutputNode buildQueryLogicalPlan(
  Query query,
  Optional<Sink> sink,
  MetaStore metaStore,
  KsqlConfig config,
  boolean rowpartitionRowoffsetEnabled)
```

`buildQueryLogicalPlan`...FIXME

`buildQueryLogicalPlan` is used when:

* `EngineExecutor` is requested to [planQuery](EngineExecutor.md#planQuery)
