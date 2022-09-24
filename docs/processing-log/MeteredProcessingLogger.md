# MeteredProcessingLogger

`MeteredProcessingLogger` is a [ProcessingLogger](ProcessingLogger.md).

## Creating Instance

`MeteredProcessingLogger` takes the following to be created:

* <span id="logger"> [ProcessingLogger](ProcessingLogger.md)
* <span id="metrics"> `Metrics`
* <span id="errorSensor"> Error `Sensor`

`MeteredProcessingLogger` is created when:

* `MeteredProcessingLoggerFactory` is requested for a [ProcessingLogger](MeteredProcessingLoggerFactory.md#getLogger)
