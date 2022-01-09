# SandboxedExecutionContext

`SandboxedExecutionContext` is a [KsqlExecutionContext](KsqlExecutionContext.md) for executing SQL statements without affecting the state of the system (i.e. no changes to the core engine's state nor the state of external services).

## Creating Instance

`SandboxedExecutionContext` takes the following to be created:

* <span id="sourceContext"> [EngineContext](EngineContext.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="metricCollectors"> `MetricCollectors`

`SandboxedExecutionContext` is created when:

* `KsqlEngine` is requested to [create a sandboxed execution context](KsqlEngine.md#createSandbox)
* `SandboxedExecutionContext` is requested to [create a sandboxed execution context](#createSandbox)

---

`SandboxedExecutionContext` is a `KsqlExecutionContext` and part of the abstraction is to [create a SandboxedExecutionContext](KsqlExecutionContext.md#createSandbox).
This is exactly `SandboxedExecutionContext` itself by default.

That's why instances of `SandboxedExecutionContext`s are created indirectly via [KsqlExecutionContext](KsqlExecutionContext.md#createSandbox).

## <span id="engineContext"> EngineContext

While being [created](#creating-instance), `SandboxedExecutionContext` requests the given [source EngineContext](#sourceContext) to [create a sandboxed EngineContext](EngineContext.md#createSandbox).

## <span id="plan"> Statement Planning (plan)

```java
KsqlPlan plan(
  ServiceContext serviceContext,
  ConfiguredStatement<?> statement)
```

`plan` [creates an EngineExecutor](EngineExecutor.md#create) to [plan](EngineExecutor.md#plan) the given `ConfiguredStatement`.

`plan` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#plan) abstraction.

## <span id="execute"> Executing Statement

```java
ExecuteResult execute(
  ServiceContext serviceContext,
  ConfiguredStatement<?> statement) // (1)
ExecuteResult execute(
  ServiceContext serviceContext,
  ConfiguredKsqlPlan plan)
```

1. [Plans the statement](#plan) and creates a `ConfiguredKsqlPlan` for the other `execute`

`execute` [creates an EngineExecutor](EngineExecutor.md#create) to [execute](EngineExecutor.md#execute) the [KsqlPlan](KsqlPlan.md) (of the `ConfiguredKsqlPlan`) and produce an `ExecuteResult`.

`execute` requests the `ExecuteResult` for the [QueryMetadata](QueryMetadata.md) and [get the KafkaStreams client](QueryMetadata.md#getKafkaStreams) that is closed right after.

In the end, `execute` returns the `ExecuteResult`.

`execute` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#execute) abstraction.

## <span id="executeTransientQuery"> Executing Transient Query

```java
TransientQueryMetadata executeTransientQuery(
  ServiceContext serviceContext,
  ConfiguredStatement<Query> statement,
  boolean excludeTombstones)
```

`executeTransientQuery` [creates an EngineExecutor](EngineExecutor.md#create) to [executeTransientQuery](EngineExecutor.md#executeTransientQuery).

`executeTransientQuery` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#executeTransientQuery) abstraction.
