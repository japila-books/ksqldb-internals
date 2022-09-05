# TransientQueryMetadata

`TransientQueryMetadata` is a [QueryMetadataImpl](QueryMetadataImpl.md) and a [PushQueryMetadata](PushQueryMetadata.md).

## Creating Instance

`TransientQueryMetadata` takes the following to be created:

* <span id="statementString"> Statement text
* <span id="logicalSchema"> `LogicalSchema`
* <span id="sourceNames"> Source Names
* <span id="executionPlan"> Execution plan
* <span id="rowQueue"> `BlockingRowQueue`
* <span id="queryId"> `QueryId`
* <span id="queryApplicationId"> Query Application ID
* <span id="topology"> `Topology` ([Kafka Streams]({{ book.kafka_streams }}/Topology/))
* <span id="kafkaStreamsBuilder"> [KafkaStreamsBuilder](KafkaStreamsBuilder.md)
* <span id="streamsProperties"> Streams Properties
* <span id="overriddenProperties"> Overridden Properties
* <span id="closeTimeout"> Close timeout
* <span id="maxQueryErrorsQueueSize"> `maxQueryErrorsQueueSize`
* <span id="resultType"> `ResultType`
* <span id="retryBackoffInitialMs"> `retryBackoffInitialMs`
* <span id="retryBackoffMaxMs"> `retryBackoffMaxMs`
* <span id="listener"> `Listener`
* <span id="loggerFactory"> `ProcessingLoggerFactory`

`TransientQueryMetadata` is created when:

* `QueryBuilder` is requested to [build a transient query](QueryBuilder.md#buildTransientQuery)
* `SandboxedTransientQueryMetadata` is created

## <span id="getQueryType"> Query Type

```java
KsqlQueryType getQueryType()
```

`getQueryType` is part of the [QueryMetadata](QueryMetadata.md#getQueryType) abstraction.

---

`getQueryType` is [KsqlQueryType.PUSH](KsqlQueryType.md#PUSH).
