# KafkaSerdeFactory

## <span id="SERDE"> Serdes Registry

`KafkaSerdeFactory` defines `SERDE` registry of `Serdes` ([Apache Kafka]({{ book.kafka }}/Serdes)) that are [supported](#supportsKeyType) by [KafkaFormat](KafkaFormat.md).

Type | Serde
-----|------
 `Integer` | `Serdes.Integer`
 `Long` | `Serdes.Long`
 `Double` | `Serdes.Double`
 `String` | `Serdes.String`
 `ByteBuffer` | `Serdes.ByteBuffer`

### <span id="containsSerde"> containsSerde

```java
boolean containsSerde(
  Class<?> javaType)
```

`containsSerde` is `true` when the given `javaType` is in [SERDE](#SERDE) registry.

---

`containsSerde` is used when:

* `KafkaFormat` is requested to [supportsKeyType](KafkaFormat.md#supportsKeyType)

## <span id="createSerde"> Creating Serde

```java
Serde<List<?>> createSerde(
  PersistenceSchema schema)
```

`createSerde` returns a `KsqlVoidSerde` for no columns in the given `PersistenceSchema`.

`createSerde` throws a `KsqlException` for two or more columns in the given `PersistenceSchema`:

```text
The 'KAFKA' format only supports a single field. Got: [columns]
```

`createSerde` converts the SQL type of the single column (in the schema) to a Java type and [creates a Serde](#createSerde-column-javatype).

---

`createSerde` is used when:

* `KafkaFormat` is requested for a [Serde for a given schema](KafkaFormat.md#getSerde)

### <span id="createSerde-column-javatype"> createSerde

```java
<T> Serde<List<?>> createSerde(
  SimpleColumn singleColumn,
  Class<T> javaType)
```

`createSerde`...FIXME
