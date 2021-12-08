# KafkaStreamsBuilderImpl

`KafkaStreamsBuilderImpl` is a [KafkaStreamsBuilder](KafkaStreamsBuilder.md).

`KafkaStreamsBuilderImpl` acts as a bridge between ksqlDB and [Kafka Streams]({{ book.kafka_streams }}) library.

## Creating Instance

`KafkaStreamsBuilderImpl` takes the following to be created:

* [KafkaClientSupplier](#clientSupplier)

`KafkaStreamsBuilderImpl` is created along with a [QueryBuilder](QueryBuilder.md#kafkaStreamsBuilder).

## <span id="clientSupplier"> KafkaClientSupplier

`KafkaStreamsBuilderImpl` is given a `KafkaClientSupplier` ([Kafka Streams]({{ book.kafka_streams }}/KafkaClientSupplier)) when [created](#creating-instance).

## <span id="build"> Building KafkaStreams Client

```java
KafkaStreams build(
  Topology topology, 
  Map<String, Object> conf)
```

`build` creates a `KafkaStreams` ([Kafka Streams]({{ book.kafka_streams }}/KafkaStreams)) with the given `Topology` ([Kafka Streams]({{ book.kafka_streams }}/Topology)) (with the given `conf` properties and the [KafkaClientSupplier](#clientSupplier)).

`build` is part of the [KafkaStreamsBuilder](KafkaStreamsBuilder.md#build) abstraction.

## <span id="buildNamedTopologyWrapper"> buildNamedTopologyWrapper

```java
KafkaStreamsNamedTopologyWrapper buildNamedTopologyWrapper(
  Map<String, Object> conf)
```

`buildNamedTopologyWrapper` creates a `KafkaStreamsNamedTopologyWrapper` with the given `conf` properties and the [KafkaClientSupplier](#clientSupplier).

`buildNamedTopologyWrapper` is part of the [KafkaStreamsBuilder](KafkaStreamsBuilder.md#buildNamedTopologyWrapper) abstraction.
