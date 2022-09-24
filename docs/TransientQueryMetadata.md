# TransientQueryMetadata

`TransientQueryMetadata` is a [QueryMetadataImpl](QueryMetadataImpl.md) and a [PushQueryMetadata](PushQueryMetadata.md).

## Creating Instance

`TransientQueryMetadata` takes the following to be created:

* <span id="statementString"> Statement text
* <span id="logicalSchema"> [LogicalSchema](LogicalSchema.md)
* <span id="sourceNames"> Source Names
* <span id="executionPlan"> Execution Plan
* <span id="rowQueue"> `BlockingRowQueue`
* <span id="queryId"> Query ID
* <span id="queryApplicationId"> Query Application ID
* <span id="topology"> `Topology` ([Kafka Streams]({{ book.kafka_streams }}/Topology/))
* <span id="kafkaStreamsBuilder"> [KafkaStreamsBuilder](KafkaStreamsBuilder.md)
* <span id="streamsProperties"> Streams Properties
* <span id="overriddenProperties"> Overridden Properties
* <span id="closeTimeout"> [ksql.streams.shutdown.timeout.ms](KsqlConfig.md#ksql.streams.shutdown.timeout.ms)
* <span id="maxQueryErrorsQueueSize"> [ksql.query.error.max.queue.size](KsqlConfig.md#KSQL_QUERY_ERROR_MAX_QUEUE_SIZE)
* <span id="resultType"> `ResultType`
* <span id="retryBackoffInitialMs"> [ksql.query.retry.backoff.initial.ms](KsqlConfig.md#KSQL_QUERY_RETRY_BACKOFF_INITIAL_MS)
* <span id="retryBackoffMaxMs"> [ksql.query.retry.backoff.max.ms](KsqlConfig.md#KSQL_QUERY_RETRY_BACKOFF_MAX_MS)
* <span id="listener"> `Listener`
* [ProcessingLoggerFactory](#loggerFactory)

`TransientQueryMetadata` is created when:

* `QueryBuilder` is requested to [build a transient query](QueryBuilder.md#buildTransientQuery)
* `SandboxedTransientQueryMetadata` is created

### <span id="loggerFactory"> ProcessingLoggerFactory

`TransientQueryMetadata` is given a [ProcessingLoggerFactory](processing-log/ProcessingLoggerFactory.md) when [created](#creating-instance).

The `ProcessingLoggerFactory` is part of [QueryMetadataImpl](QueryMetadataImpl.md#loggerFactory) abstraction.

## <span id="getQueryType"> Query Type

```java
KsqlQueryType getQueryType()
```

`getQueryType` is part of the [QueryMetadata](QueryMetadata.md#getQueryType) abstraction.

---

`getQueryType` is [KsqlQueryType.PUSH](KsqlQueryType.md#PUSH).
