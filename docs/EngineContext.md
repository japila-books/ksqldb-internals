# EngineContext

## Creating Instance

`EngineContext` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="processingLogContext"> `ProcessingLogContext`
* <span id="metaStore"> `MutableMetaStore`
* <span id="queryIdGenerator"> `QueryIdGenerator`
* <span id="parser"> [KsqlParser](KsqlParser.md)
* <span id="cleanupService"> `QueryCleanupService`
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="queryRegistry"> [QueryRegistry](QueryRegistry.md)

`EngineContext` is created using [create](#create) and [createSandbox](#createSandbox) factories.

## <span id="ddlCommandFactory"> CommandFactories

`EngineContext` creates a [CommandFactories](CommandFactories.md) when [created](#creating-instance).

The `CommandFactories` is used to [create a DdlCommand](#createDdlCommand).

## <span id="parse"> Parsing SQL Statements

```java
List<ParsedStatement> parse(
  String sql)
```

`parse` requests the [KsqlParser](#parser) to [parse the given SQL statements](KsqlParser.md#parse).

`parse` is used when:

* `EngineContext` is requested to [substituteVariables](#substituteVariables)
* `KsqlEngine` is requested to [parse a SQL text](KsqlEngine.md#parse)
* `SandboxedExecutionContext` is requested to `parse`

## <span id="prepare"> prepare

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
```

`prepare` requests the [KsqlParser](#parser) to [prepare the ParsedStatement](KsqlParser.md#parse) and creates a new `PreparedStatement`.

`prepare` is used when:

* `KsqlEngine` is requested to [prepare a ParsedStatement](KsqlEngine.md#prepare)
* `SandboxedExecutionContext` is requested to `prepare` a `ParsedStatement`

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

`create` creates a [EngineContext](#creating-instance) (with a new [DefaultKsqlParser](DefaultKsqlParser.md), a new [QueryRegistryImpl](QueryRegistryImpl.md) and the others).

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

* `EngineExecutor` is requested to [plan a ExecutableDdlStatement](EngineExecutor.md#plan) and [maybeCreateSinkDdl](EngineExecutor.md#maybeCreateSinkDdl)
