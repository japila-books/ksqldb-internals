# EngineContext

## Creating Instance

`EngineContext` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="processingLogContext"> `ProcessingLogContext`
* <span id="metaStore"> `MutableMetaStore`
* <span id="queryIdGenerator"> `QueryIdGenerator`
* <span id="parser"> [KsqlParser](parser/KsqlParser.md)
* <span id="cleanupService"> `QueryCleanupService`
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* [QueryRegistry](#queryRegistry)
* <span id="runtimeAssignor"> `RuntimeAssignor`

`EngineContext` is created using [create](#create) and [createSandbox](#createSandbox) factories.

## <span id="queryRegistry"> QueryRegistry

`EngineContext` is given a [QueryRegistry](QueryRegistry.md) when [created](#creating-instance).

The `QueryRegistry` is used when:

* [createSandbox](#createSandbox)
* [maybeTerminateCreateAsQuery](#maybeTerminateCreateAsQuery)
* [throwIfInsertQueriesExist](#throwIfInsertQueriesExist)

### <span id="getQueryRegistry"> getQueryRegistry

```java
QueryRegistry getQueryRegistry()
```

`getQueryRegistry` is used when:

* `EngineExecutor` is requested to [executeTransientQuery](EngineExecutor.md#executeTransientQuery), [executeStreamPullQuery](EngineExecutor.md#executeStreamPullQuery), [sourceTablePlan](EngineExecutor.md#sourceTablePlan), [plan a statement](EngineExecutor.md#plan), [planQuery](EngineExecutor.md#planQuery), [executePersistentQuery](EngineExecutor.md#executePersistentQuery)
* `KsqlEngine` is requested to...FIXME
* `PullQueryExecutionUtil` is requested to `findMaterializingQuery`
* `QueryIdUtil` is requested to `buildId`
* `SandboxedExecutionContext` is requested to...FIXME
* `ScalablePushQueryExecutionUtil` is requested to...FIXME

## <span id="createQueryEngine"> Creating QueryEngine

```java
QueryEngine createQueryEngine(
  ServiceContext serviceContext)
```

`createQueryEngine` creates a [QueryEngine](#creating-instance) (with the given [ServiceContext](ServiceContext.md) and the [ProcessingLogContext](#processingLogContext)).

---

`createQueryEngine` is simply a convenient factory method that does nothing but `new QueryEngine` and that little programming trick makes for a more readable fluent client code.

```java
EngineContext
  .createQueryEngine(...)
  .buildPhysicalPlan(...)
```

---

`createQueryEngine` is used when:

* `EngineExecutor` is requested to [plan a Query for execution](EngineExecutor.md#planQuery)

## <span id="ddlCommandFactory"> CommandFactories

`EngineContext` creates a [CommandFactories](CommandFactories.md) when [created](#creating-instance).

The `CommandFactories` is used to [create a DdlCommand](#createDdlCommand).

## <span id="parse"> Parsing SQL Statements

```java
List<ParsedStatement> parse(
  String sql)
```

`parse` requests the [KsqlParser](#parser) to [parse the given SQL statements](parser/KsqlParser.md#parse).

`parse` is used when:

* `EngineContext` is requested to [substituteVariables](#substituteVariables)
* `KsqlEngine` is requested to [parse a SQL text](KsqlEngine.md#parse)
* `SandboxedExecutionContext` is requested to `parse`

## <span id="prepare"> Preparing Statement for Execution

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
```

`prepare` [substitutes variables](#substituteVariables) (in the given `ParsedStatement` with the `variablesMap`) and then requests the [KsqlParser](#parser) to [prepare the ParsedStatement](parser/KsqlParser.md#parse) (with the variables resolved).

`prepare` [sanitizes the statement](AstSanitizer.md#sanitize) based on the following configuration properties (in the [KsqlConfig](#ksqlConfig)):

* [ksql.lambdas.enabled](KsqlConfig.md#KSQL_LAMBDAS_ENABLED)
* [ksql.rowpartition.rowoffset.enabled](KsqlConfig.md#KSQL_ROWPARTITION_ROWOFFSET_ENABLED)

In the end, `prepare` creates a new `PreparedStatement`.

---

`prepare` is used when:

* `KsqlEngine` is requested to [prepare a statement for execution](KsqlEngine.md#prepare)
* `SandboxedExecutionContext` is requested to [prepare a statement for execution](SandboxedExecutionContext.md#prepare)

### <span id="substituteVariables"> Variable Substitution

```java
ParsedStatement substituteVariables(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
```

`substituteVariables` [substitutes variables](parser/VariableSubstitutor.md#substitute) (in the given `ParsedStatement` with the `variablesMap`) and then [parses the SQL text](#parse).

## <span id="create"> Creating EngineContext

```java
EngineContext create(
  ServiceContext serviceContext,
  ProcessingLogContext processingLogContext,
  MutableMetaStore metaStore,
  QueryIdGenerator queryIdGenerator,
  QueryCleanupService cleanupService,
  KsqlConfig ksqlConfig,
  Collection<QueryEventListener> registrationListeners)
```

`create` creates a [EngineContext](#creating-instance) (with a new [DefaultKsqlParser](parser/DefaultKsqlParser.md), a new [QueryRegistryImpl](QueryRegistryImpl.md) and the others).

`create` is used when:

* `KsqlEngine` is [created](KsqlEngine.md#primaryContext)

## <span id="createSandbox"> Creating Sandboxed EngineContext

```java
EngineContext createSandbox(
  ServiceContext serviceContext)
```

`createSandbox` creates an [EngineContext](#creating-instance).

`createSandbox` is used when:

* `SandboxedExecutionContext` is [created](SandboxedExecutionContext.md#engineContext)

## <span id="createDdlCommand"> Creating DdlCommand

```java
DdlCommand createDdlCommand(
  KsqlStructuredDataOutputNode outputNode)
DdlCommand createDdlCommand(
  String sqlExpression,
  ExecutableDdlStatement statement,
  SessionConfig config)
```

`createDdlCommand` requests the [CommandFactories](#ddlCommandFactory) to [create a DdlCommand](CommandFactories.md#create).

`createDdlCommand` is used when:

* `EngineExecutor` is requested to [plan an ExecutableDdlStatement](EngineExecutor.md#plan) and [maybeCreateSinkDdl](EngineExecutor.md#maybeCreateSinkDdl)
