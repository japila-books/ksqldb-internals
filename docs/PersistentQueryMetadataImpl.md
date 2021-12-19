# PersistentQueryMetadataImpl

`PersistentQueryMetadataImpl` is a [QueryMetadataImpl](QueryMetadataImpl.md) and a [PersistentQueryMetadata](PersistentQueryMetadata.md).

## Creating Instance

`PersistentQueryMetadataImpl` takes the following to be created:

* <span id="persistentQueryType"> `PersistentQueryType`
* <span id="statementString"> Statement Text
* <span id="schema"> `PhysicalSchema`
* <span id="sourceNames"> Source Names
* <span id="sinkDataSource"> Sink [DataSource](DataSource.md)
* <span id="executionPlan"> Execution Plan
* <span id="id"> `QueryId`
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
* <span id="processingLogger"> `ProcessingLogger`
* <span id="retryBackoffInitialMs"> `retryBackoffInitialMs`
* <span id="retryBackoffMaxMs"> `retryBackoffMaxMs`
* <span id="listener"> `QueryMetadata.Listener`
* <span id="scalablePushRegistry"> `ScalablePushRegistry`

`PersistentQueryMetadataImpl` is created when:

* `QueryBuilder` is requested to [buildPersistentQueryInDedicatedRuntime](QueryBuilder.md#buildPersistentQueryInDedicatedRuntime)
