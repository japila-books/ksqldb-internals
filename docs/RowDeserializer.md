# RowDeserializer

`RowDeserializer` is a `Deserializer` ([Apache Kafka]({{ book.kafka }}/Deserializer)) of `java.util.List`.

## Creating Instance

`RowDeserializer` takes the following to be created:

* <span id="delegate"> Delegate `Deserializer<?>`

`RowDeserializer` is created when:

* `KafkaSerdeFactory` is requested to [create a Serde](KafkaSerdeFactory.md#createSerde)

## <span id="deserialize"> Deserializing Record

```java
List<?> deserialize(
  String topic,
  byte[] bytes)
```

`deserialize` is part of the `Deserializer` ([Apache Kafka]({{ book.kafka }}/Deserializer#deserialize)) abstraction.

---

`deserialize` requests the [delegate Deserializer](#delegate) to `deserialize` the given `bytes`.

If the deserialized value is `null`, `deserialize` returns `null`.

Otherwise, `deserialize` returns a single-element collection with the deserialized value.

In case of any exception while deserializing the `bytes`, `deserialize` throws a `SerializationException`:

```text
Error deserializing KAFKA message from topic: [topic]
```
