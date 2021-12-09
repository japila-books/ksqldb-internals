# EngineExecutor

## <span id="executeTransientQuery"> executeTransientQuery

```java
TransientQueryMetadata executeTransientQuery(
  ConfiguredStatement<Query> statement,
  boolean excludeTombstones)
```

`executeTransientQuery`...FIXME

In the end, `executeTransientQuery` requests the [EngineContext](#engineContext) for the [QueryRegistry](EngineContext.md#getQueryRegistry) that is in turn requested to [createTransientQuery](QueryRegistry.md#createTransientQuery).

`executeTransientQuery` is used when:

* `KsqlEngine` is requested to [executeTransientQuery](KsqlEngine.md#executeTransientQuery)
* `SandboxedExecutionContext` is requested to [executeTransientQuery](SandboxedExecutionContext.md#executeTransientQuery)

## <span id="planQuery"> planQuery

```java
ExecutorPlans planQuery(
  ConfiguredStatement<?> statement,
  Query query,
  Optional<Sink> sink,
  Optional<String> withQueryId,
  MetaStore metaStore)
```

`planQuery`...FIXME

`planQuery` is used when:

* `EngineExecutor` is requested to [executeTransientQuery](#executeTransientQuery), [executeStreamPullQuery](#executeStreamPullQuery), [sourceTablePlan](#sourceTablePlan), [plan](#plan)
