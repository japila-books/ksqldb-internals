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

## <span id="executeTablePullQuery"> executeTablePullQuery

```java
PullQueryResult executeTablePullQuery(
  ImmutableAnalysis analysis,
  ConfiguredStatement<Query> statement,
  HARouting routing,
  RoutingOptions routingOptions,
  QueryPlannerOptions queryPlannerOptions,
  Optional<PullQueryExecutorMetrics> pullQueryMetrics,
  boolean startImmediately,
  Optional<ConsistencyOffsetVector> consistencyOffsetVector)
```

`executeTablePullQuery`...FIXME

`executeTablePullQuery` is used when:

* `KsqlEngine` is requested to [executeTablePullQuery](KsqlEngine.md#executeTablePullQuery)
* `SandboxedExecutionContext` is requested to [executeTablePullQuery](SandboxedExecutionContext.md#executeTablePullQuery)

## <span id="buildAndValidateLogicalPlan"> buildAndValidateLogicalPlan

```java
LogicalPlanNode buildAndValidateLogicalPlan(
  ConfiguredStatement<?> statement,
  ImmutableAnalysis analysis,
  KsqlConfig config,
  QueryPlannerOptions queryPlannerOptions,
  boolean isScalablePush)
```

`buildAndValidateLogicalPlan` creates a [LogicalPlanner](LogicalPlanner.md) to [buildQueryLogicalPlan](LogicalPlanner.md#buildQueryLogicalPlan) (that gives an `OutputNode`).

In the end, `buildAndValidateLogicalPlan` creates a `LogicalPlanNode` (with the statement of the given `ConfiguredStatement` and the `OutputNode`).

`buildAndValidateLogicalPlan` is used when:

* `EngineExecutor` is requested to [executeTablePullQuery](#executeTablePullQuery) (with the `isScalablePush` flag disabled) and [executeScalablePushQuery](#executeScalablePushQuery) (with the `isScalablePush` flag enabled)

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

## <span id="plan"> Query Planning

```java
KsqlPlan plan(
  ConfiguredStatement<?> statement)
```

`plan` requests the given `ConfiguredStatement` for the [Statement](Statement.md).

For a [ExecutableDdlStatement](ExecutableDdlStatement.md), `plan` determines whether it is a [CreateStream](CreateStream.md) or a `CreateTable`. They are supposed to be a [source](CreateSource.md#isSource).

For a source `CreateTable`, `plan` [sourceTablePlan](#sourceTablePlan) (with the statement). Otherwise, `plan` requests the [EngineContext](#engineContext) to [create a DdlCommand](EngineContext.md#createDdlCommand) and then [creates a KsqlPlanV1](KsqlPlan.md#ddlPlanCurrent).

Otherwise, `plan`...FIXME

---

`plan` [throws a KsqlStatementException for a non-executable statement](#throwOnNonExecutableStatement).

`plan` throws a `KsqlStatementException` for a [CreateStream](CreateStream.md) or a `CreateTable` that are [source](CreateSource.md#isSource) the [ksql.source.table.materialization.enabled](#isSourceTableMaterializationEnabled) configuration property is disabled:

```text
Cannot execute command because source table materialization is disabled.
```

---

`plan` is used when:

* `KsqlEngine` is requested to [plan](KsqlEngine.md#plan)
* `SandboxedExecutionContext` is requested to [plan](SandboxedExecutionContext.md#plan)
