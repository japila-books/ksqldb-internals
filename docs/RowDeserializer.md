# RowDeserializer

`RowDeserializer` is a `Deserializer` (Apache Kafka) of `java.util.List`.

## Creating Instance

`RowDeserializer` takes the following to be created:

* <span id="delegate"> `Deserializer<?>`

`RowDeserializer` is created when:

* `KafkaSerdeFactory` is requested to [createSerde](KafkaSerdeFactory.md#createSerde)
