# SourceStep

`SourceStep<K>` is an extension of the [ExecutionStep](ExecutionStep.md) abstraction for [source execution steps](#implementations).

## Implementations

* [StreamSource](StreamSource.md)
* `TableSource`
* `TableSourceV1`
* `WindowedStreamSource`
* `WindowedTableSource`

## Creating Instance

`SourceStep` takes the following to be created:

* <span id="properties"> `ExecutionStepPropertiesV1`
* <span id="topicName"> Topic Name
* <span id="formats"> `Formats`
* <span id="timestampColumn"> `TimestampColumn`
* <span id="sourceSchema"> `LogicalSchema`
* <span id="pseudoColumnVersion"> Pseudo Column Version

!!! note "Abstract Class"
    `SourceStep` is an abstract class and cannot be created directly. It is created indirectly for the [concrete SourceSteps](#implementations).
