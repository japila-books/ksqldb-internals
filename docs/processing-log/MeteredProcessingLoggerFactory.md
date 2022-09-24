# MeteredProcessingLoggerFactory

`MeteredProcessingLoggerFactory` is a [ProcessingLoggerFactory](ProcessingLoggerFactory.md).

## Creating Instance

`MeteredProcessingLoggerFactory` takes the following to be created:

* <span id="config"> [ProcessingLogConfig](ProcessingLogConfig.md)
* <span id="innerFactory"> `StructuredLoggerFactory`
* <span id="metrics"> `Metrics`
* <span id="loggerFactory"> [ProcessingLogger](ProcessingLogger.md) factory
* <span id="loggerWithMetricsFactory"> [MeteredProcessingLogger](MeteredProcessingLogger.md) factory
* <span id="metricsTags"> Metrics Tags

`MeteredProcessingLoggerFactory` is created when:

* `ProcessingLogContextImpl` is [created](ProcessingLogContextImpl.md#loggerFactory)

## <span id="getLogger"> Create ProcessingLogger

```java
ProcessingLogger getLogger(
  String name,
  Map<String, String> additionalTags)
```

`getLogger` is part of the [ProcessingLoggerFactory](ProcessingLoggerFactory.md#getLogger) abstraction.

---

`getLogger` finds a [ProcessingLogger](ProcessingLogger.md) in the [processingLoggers](#processingLoggers) registry or creates a new one (using the [loggerWithMetricsFactory](#loggerWithMetricsFactory)) if not found.

When creating a [ProcessingLogger](ProcessingLogger.md), `getLogger` [configureProcessingErrorSensor](#configureProcessingErrorSensor) and [getProcessLogger](#getProcessLogger).

### <span id="getProcessLogger"> getProcessLogger

```java
ProcessingLogger getProcessLogger(
  String name)
```

`getProcessLogger` creates a [ProcessingLogger](ProcessingLogger.md) using the [loggerFactory](#loggerFactory) (with the [ProcessingLogConfig](#config) and a `StructuredLogger` from the [StructuredLoggerFactory](#innerFactory)).
