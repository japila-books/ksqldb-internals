# ProcessingLoggerImpl

`ProcessingLoggerImpl` is a [ProcessingLogger](ProcessingLogger.md) that uses [StructuredLogger](#inner) to [log error messages](#error).

## Creating Instance

`ProcessingLoggerImpl` takes the following to be created:

* <span id="config"> [ProcessingLogConfig](ProcessingLogConfig.md)
* <span id="inner"> `StructuredLogger`

`ProcessingLoggerImpl` is created when:

* `MeteredProcessingLoggerFactory` is [created](MeteredProcessingLoggerFactory.md#loggerFactory)

## <span id="error"> error

```java
void error(
  ErrorMessage msg)
```

`error` is part of the [ProcessingLogger](ProcessingLogger.md#error) abstraction.

---

`error` requests the [inner StructuredLogger](#inner) to [log the error message](#error).
