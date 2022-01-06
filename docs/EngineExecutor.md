# EngineExecutor

## Creating Instance

`EngineExecutor` takes the following to be created:

* <span id="engineContext"> [EngineContext](EngineContext.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="config"> [SessionConfig](SessionConfig.md)

`EngineExecutor` is created using [create](#create) factory.

### <span id="create"> Creating EngineExecutor

```java
EngineExecutor create(
  EngineContext engineContext,
  ServiceContext serviceContext,
  SessionConfig config)
```

`create` creates an [EngineExecutor](#creating-instance).

---

`create` is simply a convenient static factory method that does nothing but `new EngineExecutor` and that little programming trick makes for a more readable fluent client code.

```java
EngineExecutor
  .create(...)
  .plan(statement)
```

---

`create` is used when:

* `KsqlEngine` is requested to [plan](KsqlEngine.md#plan), [execute](KsqlEngine.md#execute), [executeTransientQuery](KsqlEngine.md#executeTransientQuery), [createStreamPullQuery](KsqlEngine.md#createStreamPullQuery), [executeScalablePushQuery](KsqlEngine.md#executeScalablePushQuery), [executeTablePullQuery](KsqlEngine.md#executeTablePullQuery)
* `SandboxedExecutionContext` is requested to [plan](SandboxedExecutionContext.md#plan), [execute](SandboxedExecutionContext.md#execute), [executeTransientQuery](SandboxedExecutionContext.md#executeTransientQuery), [executeTablePullQueryQuery](SandboxedExecutionContext.md#executeTablePullQueryQuery), [executeScalablePushQuery](SandboxedExecutionContext.md#executeScalablePushQuery)

## <span id="planQuery"> Planning Query for Execution (planQuery)

```java
ExecutorPlans planQuery(
  ConfiguredStatement<?> statement,
  Query query,
  Optional<Sink> sink,
  Optional<String> withQueryId,
  MetaStore metaStore)
```

`planQuery` takes a [Query](parser/Query.md) and [creates an ExecutorPlans](#planQuery-PhysicalPlan).

The optional `Sink` and query ID are only given when `EngineExecutor` is requested to [plan a QueryContainer](#plan). Otherwise, they are both undefined.

---

`planQuery` is used when:

* `EngineExecutor` is requested to execute [transient](#executeTransientQuery) and [stream pull](#executeStreamPullQuery) queries, and to [plan a statement](#plan) (and [sourceTablePlan](#sourceTablePlan))

### <span id="planQuery-QueryEngine"> Step 1. Creating QueryEngine

`planQuery` requests the [EngineContext](#engineContext) to [create a QueryEngine](EngineContext.md#createQueryEngine) (for the [ServiceContext](#serviceContext)).

!!! note
    `planQuery` creates a [QueryEngine](QueryEngine.md) every time it is executed.

### <span id="planQuery-OutputNode"> Step 2. Building Logical Plan

`planQuery` [builds a logical query plan](QueryEngine.md#buildQueryLogicalPlan) (an [OutputNode](planner/OutputNode.md)).

`planQuery` creates a `LogicalPlanNode` (for the query statement text and the `OutputNode`), a `QueryId` and looks up a `PersistentQueryMetadata` (in [QueryRegistry](EngineContext.md#getQueryRegistry)) for the `QueryId` (if one exists).

### <span id="planQuery-PhysicalPlan"> Step 3. Building Physical Plan

`planQuery` [builds a physical query plan](QueryEngine.md#buildPhysicalPlan) (a [PhysicalPlan](PhysicalPlan.md)).

### <span id="planQuery-PhysicalPlan"> Step 4. Creating ExecutorPlans

In the end, `planQuery` creates an `ExecutorPlans` (with the `LogicalPlanNode` and the `PhysicalPlan`).

## <span id="plan"> Planning ExecutableDdlStatement or QueryContainer (plan)

```java
KsqlPlan plan(
  ConfiguredStatement<?> statement)
```

`plan` requests the given `ConfiguredStatement` for the [Statement](parser/Statement.md).

### <span id="plan-ExecutableDdlStatement"> ExecutableDdlStatement

For a [ExecutableDdlStatement](parser/ExecutableDdlStatement.md), `plan` determines whether it is a [CreateStream](parser/CreateStream.md) or a [CreateTable](parser/CreateTable.md). They are both supposed to be a [source](parser/CreateSource.md#isSource).

For a source `CreateTable`, `plan` [sourceTablePlan](#sourceTablePlan). Otherwise, `plan` requests the [EngineContext](#engineContext) to [create a DdlCommand](EngineContext.md#createDdlCommand) and then [creates a KsqlPlanV1](KsqlPlan.md#ddlPlanCurrent).

### <span id="plan-QueryContainer"> QueryContainer

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

### <span id="maybeCreateSinkDdl"> maybeCreateSinkDdl

```java
Optional<DdlCommand> maybeCreateSinkDdl(
  ConfiguredStatement<?> cfgStatement,
  KsqlStructuredDataOutputNode outputNode)
```

`maybeCreateSinkDdl` returns an empty value for a [KsqlStructuredDataOutputNode](planner/KsqlStructuredDataOutputNode.md) with no [createInto](planner/KsqlStructuredDataOutputNode.md#createInto). `maybeCreateSinkDdl`  [validateExistingSink](#validateExistingSink).

Otherwise, `maybeCreateSinkDdl` requests the [EngineContext](#engineContext) to [createDdlCommand](EngineContext.md#createDdlCommand) for the given [KsqlStructuredDataOutputNode](planner/KsqlStructuredDataOutputNode.md).

### <span id="sourceTablePlan"> sourceTablePlan

```java
KsqlPlan sourceTablePlan(
  ConfiguredStatement<?> statement)
```

`sourceTablePlan` assumes that the given `ConfiguredStatement` is for a [CreateTable](parser/CreateTable.md).

`sourceTablePlan`...FIXME

### <span id="plan-usage"> Usage

`plan` is used when:

* `KsqlEngine` is requested to [plan a query](KsqlEngine.md#plan)
* `SandboxedExecutionContext` is requested to [plan a query](SandboxedExecutionContext.md#plan)

## <span id="execute"> Executing KsqlPlan (execute)

```java
ExecuteResult execute(
  KsqlPlan plan)
```

!!! note "KsqlPlan"
    It is an `IllegalStateException` if the given [KsqlPlan](KsqlPlan.md) has neither a [physical plan](KsqlPlan.md#getQueryPlan) nor a [DdlCommand](KsqlPlan.md#getDdlCommand).

For the given [KsqlPlan](KsqlPlan.md) with no [physical plan](KsqlPlan.md#getQueryPlan), `execute` [executes the DDL command](#executeDdl) (of the [KsqlPlan](KsqlPlan.md#getDdlCommand) with the `withQuery` flag off) and returns.

Otherwise, with a [physical plan](KsqlPlan.md#getQueryPlan), `execute` [executes a DDL command](#executeDdl) (if available) and [executes the persistent query](#executePersistentQuery).

---

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

## <span id="executeScalablePushQuery"> Executing Scalable Push Query

```java
ScalablePushQueryMetadata executeScalablePushQuery(
  ImmutableAnalysis analysis,
  ConfiguredStatement<Query> statement,
  PushRouting pushRouting,
  PushRoutingOptions pushRoutingOptions,
  QueryPlannerOptions queryPlannerOptions,
  Context context,
  Optional<ScalablePushQueryMetrics> scalablePushQueryMetrics)
```

`executeScalablePushQuery`...FIXME

`executeScalablePushQuery` is used when:

* `KsqlEngine` is requested to [executeScalablePushQuery](KsqlEngine.md#executeScalablePushQuery)
* `SandboxedExecutionContext` is requested to [executeScalablePushQuery](SandboxedExecutionContext.md#executeScalablePushQuery)

## <span id="executeStreamPullQuery"> Executing Stream Pull Query

```java
TransientQueryMetadata executeStreamPullQuery(
  ConfiguredStatement<Query> statement,
  boolean excludeTombstones,
  ImmutableMap<TopicPartition, Long> endOffsets)
```

`executeStreamPullQuery`...FIXME

`executeStreamPullQuery` is used when:

* `KsqlEngine` is requested to [create a stream pull query](KsqlEngine.md#createStreamPullQuery)

## <span id="executeTransientQuery"> Executing Transient Query

```java
TransientQueryMetadata executeTransientQuery(
  ConfiguredStatement<Query> statement,
  boolean excludeTombstones)
```

`executeTransientQuery`...FIXME

In the end, `executeTransientQuery` requests the [EngineContext](#engineContext) for the [QueryRegistry](EngineContext.md#getQueryRegistry) to [create a transient query](QueryRegistry.md#createTransientQuery).

`executeTransientQuery` is used when:

* `KsqlEngine` is requested to [execute a transient query](KsqlEngine.md#executeTransientQuery)
* `SandboxedExecutionContext` is requested to [execute a transient query](SandboxedExecutionContext.md#executeTransientQuery)

## <span id="executeTablePullQuery"> Executing Table Pull Query

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

## <span id="buildAndValidateLogicalPlan"> Building Logical Plan of Query (buildAndValidateLogicalPlan)

```java
LogicalPlanNode buildAndValidateLogicalPlan(
  ConfiguredStatement<?> statement,
  ImmutableAnalysis analysis,
  KsqlConfig config,
  QueryPlannerOptions queryPlannerOptions,
  boolean isScalablePush)
```

`buildAndValidateLogicalPlan` is given a statement (along with the [analysis](ImmutableAnalysis.md)) and the `isScalablePush` flag as follows:

* `true` when [executing a scalable push query](#executeScalablePushQuery)
* `false` when [executing a table pull query](#executeTablePullQuery)

!!! note "Naming"
    Since `buildAndValidateLogicalPlan` is to execute [buildQueryLogicalPlan](planner/LogicalPlanner.md#buildQueryLogicalPlan) it'd make sense to call it alike, _wouldn't it?_

    After all, `buildAndValidateLogicalPlan` is a facade to [LogicalPlanner](planner/LogicalPlanner.md).

---

`buildAndValidateLogicalPlan` creates a [LogicalPlanner](planner/LogicalPlanner.md) to [build a logical plan of a query](planner/LogicalPlanner.md#buildQueryLogicalPlan) (that gives an [OutputNode](planner/OutputNode.md)).

In the end, `buildAndValidateLogicalPlan` creates a `LogicalPlanNode` (with the statement text of the given `ConfiguredStatement` and the [OutputNode](planner/OutputNode.md)).

---

`buildAndValidateLogicalPlan` is used when:

* `EngineExecutor` is requested to execute [table pull](#executeTablePullQuery) or [scalable push](#executeScalablePushQuery) queries
