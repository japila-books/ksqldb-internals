# KafkaFormat

`KafkaFormat` is a [Format](Format.md) known as [KAFKA](#NAME).

`KafkaFormat` supports a [single field only in schema](KafkaSerdeFactory.md#createSerde) (does not [support multiple columns](SerdeFeaturesFactory.md#formatSupportsMultipleColumns)) and throws a `KsqlException` otherwise.

## <span id="NAME"><span id="name"> Name

The name of `KafkaFormat` is `KAFKA`.

---

Used when:

* `Analyzer.Visitor` is requested to [validate](analyzer/Analyzer.Visitor.md#validate)
* `SerdeFeaturesFactory` is requested to [formatSupportsMultipleColumns](SerdeFeaturesFactory.md#formatSupportsMultipleColumns)
* `FormatFactory` is used to [look up a Format](FormatFactory.md#fromName)

### name

```java
String name()
```

`name` is part of the [Format](Format.md#name) abstraction.

---

`name` is [KAFKA](#NAME).

## <span id="SUPPORTED_FEATURES"> Supported Serde Features

`KafkaFormat` supports `UNWRAP_SINGLES` feature.

## <span id="getSerde"> Create Serde for Schema

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

`getSerde` validates the given `formatProperties` (against the [supported properties](Format.md#getSupportedProperties) of this format that `KafkaFormat` has got none).

`getSerde` validates the given `PersistenceSchema` (against the [supported schema features](#supportedFeatures) of this format).

In the end, `getSerde` [creates a Serde](KafkaSerdeFactory.md#createSerde).

## <span id="supportsKeyType"> supportsKeyType

```java
boolean supportsKeyType(
  SqlType type)
```

`supportsKeyType` is part of the [Format](Format.md#supportsKeyType) abstraction.

---

`supportsKeyType` holds `true` if the given [SqlType](types/SqlType.md) meets the following requirements:

* It is a [SqlPrimitiveType](types/SqlPrimitiveType.md)
* There is [Serde available](KafkaSerdeFactory.md#containsSerde)
