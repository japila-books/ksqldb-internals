# ProcessingLogContext

`ProcessingLogContext` is an [abstraction](#contract) of [processing log contexts](#implementations).

## Contract

### <span id="getConfig"> getConfig

```java
ProcessingLogConfig getConfig()
```

[ProcessingLogConfig](ProcessingLogConfig.md)

See [ProcessingLogContextImpl](ProcessingLogContextImpl.md#getConfig)

### <span id="getLoggerFactory"> getLoggerFactory

```java
ProcessingLoggerFactory getLoggerFactory()
```

[ProcessingLoggerFactory](ProcessingLoggerFactory.md)

See [ProcessingLogContextImpl](ProcessingLogContextImpl.md#getLoggerFactory)

## Implementations

* `NoopProcessingLogContext`
* [ProcessingLogContextImpl](ProcessingLogContextImpl.md)

## <span id="create"> Creating ProcessingLogContext

```java
ProcessingLogContext create() // (1)!
ProcessingLogContext create(
  ProcessingLogConfig config,
  Metrics metrics,
  Map<String, String> metricsTags)
```

1. Used in [sandboxed EngineContext](../EngineContext.md#createSandbox)

`create` creates a [ProcessingLogContextImpl](ProcessingLogContextImpl.md).

---

`create` is used when:

* `EngineContext` is requested to [create a sandboxed EngineContext](../EngineContext.md#createSandbox)
* `KsqlRestApplication` is requested to [build a KsqlRestApplication](../rest/KsqlRestApplication.md#buildApplication) (and [KsqlEngine](../KsqlEngine.md#processingLogContext) and [KsqlRestApplication](../rest/KsqlRestApplication.md#processingLogContext))
* `StandaloneExecutorFactory` is requested to [create a StandaloneExecutor](../headless/StandaloneExecutorFactory.md#create) (and [KsqlEngine](../KsqlEngine.md#processingLogContext))
