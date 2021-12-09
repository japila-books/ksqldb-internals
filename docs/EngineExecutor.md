# EngineExecutor

## Creating Instance

`EngineExecutor` takes the following to be created:

* <span id="engineContext"> [EngineContext](EngineContext.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="config"> [SessionConfig](SessionConfig.md)

`EngineExecutor` is created using [create](#create) factory.

## <span id="create"> Creating EngineExecutor

```java
EngineExecutor create(
  EngineContext engineContext,
  ServiceContext serviceContext,
  SessionConfig config)
```

`create` creates an [EngineExecutor](#creating-instance).

!!! note
    `create` is simply a convenient static factory method that does nothing but `new EngineExecutor` yet makes for a more readable fluent client code.

    ```java
    EngineExecutor
      .create(...)
      .plan(statement)
    ```

`create` is used when:

* `KsqlEngine` is requested to [plan](KsqlEngine.md#plan), [execute](KsqlEngine.md#execute), [executeTransientQuery](KsqlEngine.md#executeTransientQuery), [createStreamPullQuery](KsqlEngine.md#createStreamPullQuery), [executeScalablePushQuery](KsqlEngine.md#executeScalablePushQuery), [executeTablePullQuery](KsqlEngine.md#executeTablePullQuery)
* `SandboxedExecutionContext` is requested to [plan](SandboxedExecutionContext.md#plan), [execute](SandboxedExecutionContext.md#execute), [executeTransientQuery](SandboxedExecutionContext.md#executeTransientQuery), [executeTablePullQueryQuery](SandboxedExecutionContext.md#executeTablePullQueryQuery), [executeScalablePushQuery](SandboxedExecutionContext.md#executeScalablePushQuery)

## <span id="buildAndValidateLogicalPlan"> buildAndValidateLogicalPlan

```java
LogicalPlanNode buildAndValidateLogicalPlan(
  ConfiguredStatement<?> statement,
  ImmutableAnalysis analysis,
  KsqlConfig config,
  QueryPlannerOptions queryPlannerOptions,
  boolean isScalablePush)
```

`buildAndValidateLogicalPlan`...FIXME

`buildAndValidateLogicalPlan` is used when:

* `EngineExecutor` is requested to [executeTablePullQuery](#executeTablePullQuery) and [executeScalablePushQuery](#executeScalablePushQuery)

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
