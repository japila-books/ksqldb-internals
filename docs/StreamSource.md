# StreamSource

`StreamSource` is a [SourceStep](SourceStep.md) of `KStreamHolder<GenericKey>` with a `KStream` ([Kafka Streams]({{ book.kafka_streams }}/kstream/KStream)).

## Creating Instance

`StreamSource` takes the following to be created:

* <span id="props"> `ExecutionStepPropertiesV1`
* <span id="topicName"> Topic Name
* <span id="formats"> `Formats`
* <span id="timestampColumn"> `TimestampColumn`
* <span id="sourceSchema"> `LogicalSchema`
* <span id="pseudoColumnVersion"> Pseudo Column Version

`StreamSource` is created when:

* `ExecutionStepFactory` is requested to [create a StreamSource](ExecutionStepFactory.md#streamSource)

## <span id="build"> build

```java
KStreamHolder<GenericKey> build(
  PlanBuilder builder,
  PlanInfo info)
```

`build` requests the given [PlanBuilder](PlanBuilder.md) to [visitStreamSource](PlanBuilder.md#visitStreamSource) (with this `StreamSource` and the given `PlanInfo`).

`build` is part of the [ExecutionStep](ExecutionStep.md#build) abstraction.
