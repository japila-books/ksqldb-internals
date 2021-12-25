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

`buildAndValidateLogicalPlan` creates a [LogicalPlanner](LogicalPlanner.md) to [buildQueryLogicalPlan](LogicalPlanner.md#buildQueryLogicalPlan) (that gives an [OutputNode](OutputNode.md)).

In the end, `buildAndValidateLogicalPlan` creates a `LogicalPlanNode` (with the statement of the given `ConfiguredStatement` and the [OutputNode](OutputNode.md)).

`buildAndValidateLogicalPlan` is used when:

* `EngineExecutor` is requested to [executeTablePullQuery](#executeTablePullQuery) (with the `isScalablePush` flag disabled) and [executeScalablePushQuery](#executeScalablePushQuery) (with the `isScalablePush` flag enabled)

## <span id="executeTransientQuery"> executeTransientQuery

```java
TransientQueryMetadata executeTransientQuery(
  ConfiguredStatement<Query> statement,
  boolean excludeTombstones)
```

`executeTransientQuery`...FIXME

In the end, `executeTransientQuery` requests the [EngineContext](#engineContext) for the [QueryRegistry](EngineContext.md#getQueryRegistry) to [create a transient query](QueryRegistry.md#createTransientQuery).

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

`planQuery` requests the [EngineContext](#engineContext) to [createQueryEngine](EngineContext.md#createQueryEngine) (with the [ServiceContext](#serviceContext)).

`planQuery` [buildQueryLogicalPlan](QueryEngine.md#buildQueryLogicalPlan).

`planQuery`...FIXME

`planQuery` is used when:

* `EngineExecutor` is requested to [executeTransientQuery](#executeTransientQuery), [executeStreamPullQuery](#executeStreamPullQuery), [sourceTablePlan](#sourceTablePlan), [plan](#plan)

## <span id="plan"> Query Planning (plan)

```java
KsqlPlan plan(
  ConfiguredStatement<?> statement)
```

`plan` requests the given `ConfiguredStatement` for the [Statement](parser/Statement.md).

### <span id="plan-ExecutableDdlStatement"> ExecutableDdlStatement

For a [ExecutableDdlStatement](parser/ExecutableDdlStatement.md), `plan` determines whether it is a [CreateStream](parser/CreateStream.md) or a `CreateTable`. They are supposed to be a [source](parser/CreateSource.md#isSource).

For a source `CreateTable`, `plan` [sourceTablePlan](#sourceTablePlan). Otherwise, `plan` requests the [EngineContext](#engineContext) to [create a DdlCommand](EngineContext.md#createDdlCommand) and then [creates a KsqlPlanV1](KsqlPlan.md#ddlPlanCurrent).

### <span id="plan-QueryContainer"> Planning QueryContainer

Otherwise, `plan` assumes that the `Statement` is a [QueryContainer](parser/QueryContainer.md) and [plans the query](#planQuery) (with the `Sink` among the others that gives a `PhysicalPlan`).

`plan` [maybeCreateSinkDdl](#maybeCreateSinkDdl).

`plan` creates a [QueryPlan](QueryPlan.md).

In the end, `plan` [creates a KsqlPlanV1](KsqlPlan.md#queryPlanCurrent).

### <span id="plan-exceptions"> Exceptions

`plan` [throws a KsqlStatementException for a non-executable statement](#throwOnNonExecutableStatement).

`plan` throws a `KsqlStatementException` for a [CreateStream](parser/CreateStream.md) or a `CreateTable` that are [source](parser/CreateSource.md#isSource) the [ksql.source.table.materialization.enabled](#isSourceTableMaterializationEnabled) configuration property is disabled:

```text
Cannot execute command because source table materialization is disabled.
```

### <span id="plan-usage"> Usage

`plan` is used when:

* `KsqlEngine` is requested to [plan a query](KsqlEngine.md#plan)
* `SandboxedExecutionContext` is requested to [plan a query](SandboxedExecutionContext.md#plan)

## <span id="execute"> Executing Query

```java
ExecuteResult execute(
  KsqlPlan plan)
```

`execute`...FIXME

In the end, `execute` [executes the persistent query](#executePersistentQuery).

`execute` is used when:

* `KsqlEngine` is requested to [execute a query](KsqlEngine.md#execute)
* `SandboxedExecutionContext` is requested to [execute a query](SandboxedExecutionContext.md#execute)

### <span id="executePersistentQuery"> Executing Persistent Query

```java
PersistentQueryMetadata executePersistentQuery(
  QueryPlan queryPlan,
  String statementText,
  KsqlConstants.PersistentQueryType persistentQueryType)
```

`executePersistentQuery` requests the [EngineContext](#engineContext) for the [QueryRegistry](EngineContext.md#getQueryRegistry) to [create or replace a persistent query](QueryRegistry.md#createOrReplacePersistentQuery) (for the given [QueryPlan](QueryPlan.md) and `PersistentQueryType`).
