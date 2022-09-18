# KafkaSerdeFactory

## <span id="SERDE"> Serdes Registry

`KafkaSerdeFactory` defines `SERDE` registry of `Serdes` ([Apache Kafka]({{ book.kafka }}/Serdes)).

Type | Serde
-----|------
 `Integer` | `Serdes.Integer`
 `Long` | `Serdes.Long`
 `Double` | `Serdes.Double`
 `String` | `Serdes.String`
 `ByteBuffer` | `Serdes.ByteBuffer`

## <span id="createSerde"> createSerde

```java
Serde<List<?>> createSerde(
  PersistenceSchema schema)
```

`createSerde`...FIXME

---

`createSerde` is used when:

* `KafkaFormat` is requested to [getSerde](KafkaFormat.md#getSerde)
