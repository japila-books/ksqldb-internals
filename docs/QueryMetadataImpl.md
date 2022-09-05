# QueryMetadataImpl

`QueryMetadataImpl` is a [QueryMetadata](QueryMetadata.md).

`QueryMetadataImpl` is a thin wrapper around [KafkaStreams](#kafkaStreams).

## Creating Instance

`QueryMetadataImpl` takes the following to be created:

* <span id="statementString"> Statement Text
* <span id="logicalSchema"> `LogicalSchema`
* <span id="sourceNames"> Names of the Sources
* <span id="executionPlan"> Execution Plan
* <span id="queryApplicationId"> queryApplicationId
* [Topology](#topology)
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

## <span id="topology"> Topology

`QueryMetadataImpl` is given a `Topology` ([Kafka Streams]({{ book.kafka_streams }}/Topology)) when [created](#creating-instance).

The `Topology` is used when:

* [initialize](#initialize)
* [getTopologyDescription](#getTopologyDescription)

### <span id="getTopology"> getTopology

```java
Topology getTopology()
```

`getTopology` is part of the [QueryMetadata](QueryMetadata.md#getTopology) abstraction.

---

`getTopology` returns the [Topology](#topology) instance.

## <span id="kafkaStreams"> KafkaStreams

`QueryMetadataImpl` is given a `KafkaStreams` ([Kafka Streams]({{ book.kafka_streams }}/KafkaStreams)) when [created](#creating-instance) and requested to [resetKafkaStreams](#resetKafkaStreams).

The `KafkaStreams` is started at [start](#start) and closed at [close](#close).

The `KafkaStreams` is used when:

* [getTaskMetadata](#getTaskMetadata)
* [setUncaughtExceptionHandler](#setUncaughtExceptionHandler)
* [getState](#getState)
* [getAllLocalStorePartitionLags](#getAllLocalStorePartitionLags)
* [getAllStreamsHostMetadata](#getAllStreamsHostMetadata)
* [doClose](#doClose)

### <span id="getKafkaStreams"> getKafkaStreams

```java
KafkaStreams getKafkaStreams()
```

`getKafkaStreams` is part of the [QueryMetadata](QueryMetadata.md#getKafkaStreams) abstraction.

---

`getKafkaStreams` returns the [KafkaStreams](#kafkaStreams) instance.

## <span id="kafkaStreamsBuilder"> KafkaStreamsBuilder

`QueryMetadataImpl` is given a [KafkaStreamsBuilder](KafkaStreamsBuilder.md) when [created](#creating-instance).

## <span id="initialize"> initialize

```java
void initialize()
```

`initialize` requests the [KafkaStreamsBuilder](#kafkaStreamsBuilder) to [build a KafkaStreams instance](KafkaStreamsBuilder.md#build) (with the [Topology](#topology) and the [streamsProperties](#streamsProperties)).

`initialize` [resets the KafkaStreams instance](#resetKafkaStreams) and turns the [initialized](#initialized) flag on.

`initialize` is part of the [QueryMetadata](QueryMetadata.md#initialize) abstraction.

## <span id="getQueryType"> Query Type

```java
KsqlQueryType getQueryType()
```

`getQueryType` is part of the [QueryMetadata](QueryMetadata.md#getQueryType) abstraction.

---

`getQueryType` is [KsqlQueryType.PERSISTENT](KsqlQueryType.md#PERSISTENT).

## <span id="close"> Closing Query

```java
void close()
```

`close` is part of the [QueryMetadata](QueryMetadata.md#close) abstraction.

---

`close` requests the [loggerFactory](#loggerFactory) for...FIXME

`close` [doClose](#doClose) (with `cleanUp` enabled).

In the end, `close` requests the [Listener](#listener) to `onClose`.

## <span id="doClose"> doClose

```java
void doClose(
  boolean cleanUp)
```

`doClose` [closeKafkaStreams](#closeKafkaStreams) and then requests the [KafkaStreams](#kafkaStreams) to `cleanUp`.

`doClose` prints out the following WARN message to the logs when [closeKafkaStreams](#closeKafkaStreams) did not succeed:

```text
Query has not successfully closed, skipping cleanup
```

---

`doClose` is used when:

* `PersistentQueryMetadataImpl` is requested to [stop](PersistentQueryMetadataImpl.md#stop) (with `cleanUp` disabled)
* `QueryMetadataImpl` is requested to [close](#close) (with `cleanUp` enabled)

### <span id="closeKafkaStreams"> closeKafkaStreams

```java
boolean closeKafkaStreams()
```

`closeKafkaStreams`...FIXME
