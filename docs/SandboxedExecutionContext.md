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
