# QueryMetadataImpl

`QueryMetadataImpl` is a [QueryMetadata](QueryMetadata.md).

## Creating Instance

`QueryMetadataImpl` takes the following to be created:

* <span id="statementString"> Statement Text
* <span id="logicalSchema"> `LogicalSchema`
* <span id="sourceNames"> `SourceName`s
* <span id="executionPlan"> Execution Plan
* <span id="queryApplicationId"> queryApplicationId
* <span id="topology"> `Topology`
* [KafkaStreamsBuilder](#kafkaStreamsBuilder)
* <span id="streamsProperties"> Streams Properties
* <span id="overriddenProperties"> overriddenProperties
* <span id="closeTimeout"> `closeTimeout`
* <span id="queryId"> `QueryId`
* <span id="errorClassifier"> `QueryErrorClassifier`
* <span id="maxQueryErrorsQueueSize"> `maxQueryErrorsQueueSize`
* <span id="baseWaitingTimeMs"> `baseWaitingTimeMs`
* <span id="retryBackoffMaxMs"> `retryBackoffMaxMs`
* <span id="listener"> `Listener`

## <span id="kafkaStreamsBuilder"> KafkaStreamsBuilder

`QueryMetadataImpl` is given a [KafkaStreamsBuilder](KafkaStreamsBuilder.md) when [created](#creating-instance).

## <span id="initialize"> initialize

```java
void initialize()
```

`initialize` requests the [KafkaStreamsBuilder](#kafkaStreamsBuilder) to [build a KafkaStreams instance](KafkaStreamsBuilder.md#build) (with the [Topology](#topology) and the [streamsProperties](#streamsProperties)).

`initialize` [resets the KafkaStreams instance](#resetKafkaStreams) and turns the [initialized](#initialized) flag on.

`initialize` is part of the [QueryMetadata](QueryMetadata.md#initialize) abstraction.
