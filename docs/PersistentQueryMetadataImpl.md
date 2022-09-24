# PersistentQueryMetadataImpl

`PersistentQueryMetadataImpl` is a [QueryMetadataImpl](QueryMetadataImpl.md) and a [PersistentQueryMetadata](PersistentQueryMetadata.md).

## Creating Instance

`PersistentQueryMetadataImpl` takes the following to be created:

* <span id="persistentQueryType"> `PersistentQueryType`
* <span id="statementString"> Statement Text
* <span id="schema"> [PhysicalSchema](PhysicalSchema.md)
* <span id="sourceNames"> Source Names
* <span id="sinkDataSource"> Sink [DataSource](DataSource.md)
* <span id="executionPlan"> Execution Plan
* <span id="id"> Query ID
* <span id="materializationProviderBuilder"> `MaterializationProviderBuilder`
* <span id="queryApplicationId"> Query Application ID
* <span id="topology"> `Topology` ([Kafka Streams]({{ book.kafka_streams }}/Topology))
* <span id="kafkaStreamsBuilder"> [KafkaStreamsBuilder](KafkaStreamsBuilder.md)
* <span id="schemas"> `QuerySchemas`
* <span id="streamsProperties"> Streams Properties (`Map<String, Object>`)
* <span id="overriddenProperties"> Overrideen Properties (`Map<String, Object>`)
* <span id="closeTimeout"> Close Timeout
* <span id="errorClassifier"> `QueryErrorClassifier`
* <span id="physicalPlan"> [ExecutionStep](ExecutionStep.md)
* <span id="maxQueryErrorsQueueSize"> `maxQueryErrorsQueueSize`
* [ProcessingLogger](#processingLogger)
* <span id="retryBackoffInitialMs"> `retryBackoffInitialMs`
* <span id="retryBackoffMaxMs"> `retryBackoffMaxMs`
* <span id="listener"> `QueryMetadata.Listener`
* <span id="scalablePushRegistry"> `ScalablePushRegistry`

`PersistentQueryMetadataImpl` is created when:

* `QueryBuilder` is requested to [buildPersistentQueryInDedicatedRuntime](QueryBuilder.md#buildPersistentQueryInDedicatedRuntime)

### <span id="processingLogger"><span id="getProcessingLogger"> ProcessingLogger

`PersistentQueryMetadataImpl` is given a [ProcessingLogger](processing-log/ProcessingLogger.md) when [created](#creating-instance).

The `ProcessingLogger` is used in [uncaughtHandler](#uncaughtHandler) (to [error log](processing-log/ProcessingLogger.md#error) unhandled exceptions caught in streams threads).

## <span id="uncaughtHandler"> uncaughtHandler

```java
StreamsUncaughtExceptionHandler.StreamThreadExceptionResponse uncaughtHandler(
  Throwable error)
```

`uncaughtHandler` is part of the [PersistentQueryMetadata](PersistentQueryMetadata.md#uncaughtHandler) abstraction.

---

`uncaughtHandler`...FIXME
