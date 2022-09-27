# KafkaFormat

`KafkaFormat` is a [Format](Format.md) known as [KAFKA](#NAME).

`KafkaFormat` supports a [single field only in schema](KafkaSerdeFactory.md#createSerde) (does not [support multiple columns](SerdeFeaturesFactory.md#formatSupportsMultipleColumns)).

## <span id="NAME"><span id="name"> Name

```java
String name()
```

`name` is part of the [Format](Format.md#name) abstraction.

---

`name` is `KAFKA`.

## <span id="SUPPORTED_FEATURES"><span id="supportedFeatures"> Supported Serde Features

`KafkaFormat` supports `UNWRAP_SINGLES` feature.

## <span id="getSerde"> Create Serde

```java
Serde<List<?>> getSerde(
  PersistenceSchema schema,
  Map<String, String> formatProperties,
  KsqlConfig ksqlConfig,
  Supplier<SchemaRegistryClient> srClientFactory,
  boolean isKey)
```

`getSerde` is part of the [Format](Format.md#getSerde) abstraction.

---

`getSerde` validates the given `formatProperties` against the [supported properties](Format.md#getSupportedProperties).

!!! note
    `KAFKA` format supports no custom properties in the WITH clause.

`getSerde` validates the `SerdeFeatures` (of the given `PersistenceSchema`) against the [supported schema features](#supportedFeatures).

In the end, `getSerde` [creates a Serde](KafkaSerdeFactory.md#createSerde) for the given `PersistenceSchema`.

## <span id="supportsKeyType"> supportsKeyType

```java
boolean supportsKeyType(
  SqlType type)
```

`supportsKeyType` is part of the [Format](Format.md#supportsKeyType) abstraction.

---

`supportsKeyType` holds `true` if the given [SqlType](../types/SqlType.md) meets the following requirements:

* It is a [SqlPrimitiveType](../types/SqlPrimitiveType.md)
* There is [Serde available](KafkaSerdeFactory.md#containsSerde)
