# SandboxedExecutionContext

`SandboxedExecutionContext` is a [KsqlExecutionContext](KsqlExecutionContext.md).

## Creating Instance

`SandboxedExecutionContext` takes the following to be created:

* <span id="sourceContext"> [EngineContext](EngineContext.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)

`SandboxedExecutionContext` is created when:

* `KsqlEngine` is requested to [createSandbox](KsqlEngine.md#createSandbox)
* `SandboxedExecutionContext` is requested to [createSandbox](SandboxedExecutionContext.md#createSandbox)

## <span id="engineContext"> EngineContext

While being [created](#creating-instance), `SandboxedExecutionContext` requests the given [source EngineContext](#sourceContext) to [create a sandboxed EngineContext](EngineContext.md#createSandbox).

## <span id="plan"> Query Planning (plan)

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
