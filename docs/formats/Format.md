# Format

`Format` is an [abstraction](#contract) of [serialization formats](#implementations) of a Kafka topic in ksqlDB.

## Contract

### <span id="getSerde"> Create Serde

```java
Serde<List<?>> getSerde(
  PersistenceSchema schema,
  Map<String, String> formatProperties,
  KsqlConfig ksqlConfig,
  Supplier<SchemaRegistryClient> srClientFactory,
  boolean isKey)
```

Creates a `Serde` (Apache Kafka) for the given `PersistenceSchema`

Used when:

* `GenericSerdeFactory` is requested to [create a Serde for a Format](GenericSerdeFactory.md#createFormatSerde)

### <span id="name"> name

```java
String name()
```

### <span id="supportsKeyType"> supportsKeyType

```java
boolean supportsKeyType(
  SqlType type)
```

Used when:

* `SerdeFeaturesFactory` is requested to [sanitizeKeyFormatForTypeCompatibility](SerdeFeaturesFactory.md#sanitizeKeyFormatForTypeCompatibility)

## Implementations

* `ConnectFormat`
* `DelimitedFormat`
* [KafkaFormat](KafkaFormat.md)
* `NoneFormat`
