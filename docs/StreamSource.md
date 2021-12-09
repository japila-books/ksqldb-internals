# StreamSource

`StreamSource` is a [SourceStep](SourceStep.md) of `KStreamHolder<GenericKey>`.

## Creating Instance

`StreamSource` takes the following to be created:

* <span id="props"> `ExecutionStepPropertiesV1`
* <span id="topicName"> Topic Name
* <span id="formats"> `Formats`
* <span id="timestampColumn"> `TimestampColumn`
* <span id="sourceSchema"> `LogicalSchema`
* <span id="pseudoColumnVersion"> Pseudo Column Version

`StreamSource` is created when:

* `ExecutionStepFactory` is requested to [streamSource](ExecutionStepFactory.md#streamSource)
