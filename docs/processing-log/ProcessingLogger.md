# ProcessingLogger

`ProcessingLogger` is an [abstraction](#contract) of [loggers](#implementations) to [log error messages](#error).

## Contract

### <span id="error"> Logging Error Message

```java
void error(
  ErrorMessage msg)
```

See [ProcessingLoggerImpl](ProcessingLoggerImpl.md#error)

## Implementations

* `GenericExpressionResolver`
* [MeteredProcessingLogger](MeteredProcessingLogger.md)
* `NoopProcessingLogContext`
* [ProcessingLoggerImpl](ProcessingLoggerImpl.md)
