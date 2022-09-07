# CreateSourceProperties

## Creating Instance

`CreateSourceProperties` takes the following to be created:

* <span id="originals"> Originals (`Map<String, Literal>`)
* <span id="durationParser"> Duration Parser (`Function<String, Duration>`)

`CreateSourceProperties` is created using [from](#from) factory and when:

* [withKeyValueSchemaName](#withKeyValueSchemaName)
* [withPartitions](#withPartitions)
* [withFormats](#withFormats)

While being created, `CreateSourceProperties` creates a [PropertiesConfig](#props) and performs parameter validations.

## <span id="from"> Creating CreateSourceProperties

```java
CreateSourceProperties from(
  Map<String, Literal> literals)
```

`from` creates a [CreateSourceProperties](#creating-instance) (with the given `literals` and the default `DurationParser`).

---

`from` is used when:

* `AstBuilder.Visitor` is requested to [visitCreateTable](AstBuilder_Visitor.md#visitCreateTable), [visitCreateStream](AstBuilder_Visitor.md#visitCreateStream), [visitAssertStream](AstBuilder_Visitor.md#visitAssertStream) and [visitAssertTable](AstBuilder_Visitor.md#visitAssertTable)

## <span id="props"> PropertiesConfig

When [created](#creating-instance), `CreateSourceProperties` creates a `PropertiesConfig` with the following table-specific configuration properties (in addition to [CommonCreateConfigs](CommonCreateConfigs.md)).

### <span id="SOURCE_CONNECTOR"> SOURCE_CONNECTOR

Indicates that this source was created by a connector with the given name.

Default: `null`

### <span id="WINDOW_SIZE"> WINDOW_SIZE

Window size of `HOPPING` or `TUMBLING` windows, e.g. `20 SECONDS`

Default: `null`

### <span id="WINDOW_TYPE"> WINDOW_TYPE

Supported values:

* `SESSION`
* `HOPPING`
* `TUMBLING`

Default: `null`

## <span id="getKeyFormat"> Key Format

```java
Optional<FormatInfo> getKeyFormat(
  SourceName name)
```

`getKeyFormat` determines the name of the format based on the value of [FORMAT](#getFormatName) property (if specified) or defaults to [KEY_FORMAT](CommonCreateConfigs.md#KEY_FORMAT_PROPERTY) property (from the [PropertiesConfig](#props)).

In the end, `getKeyFormat` creates a `FormatInfo` with the format name and the [key format properties](#getKeyFormatProperties) (if specified).

---

`getKeyFormat` is used when:

* `DefaultFormatInjector` is requested for `injectForCreateStatement`
* `SourcePropertiesUtil` is requested to [getKeyFormat](SourcePropertiesUtil.md#getKeyFormat)

### <span id="getKeyFormatProperties"> Key Format Properties

```java
Map<String, String> getKeyFormatProperties(
  String keyFormat,
  String name)
```

`getKeyFormatProperties`...FIXME

### <span id="getKeySchemaId"> KEY_SCHEMA_ID

```java
Optional<Integer> getKeySchemaId()
```

`getKeySchemaId` is the value of [KEY_SCHEMA_ID](CommonCreateConfigs.md#KEY_SCHEMA_ID) property.

---

`getKeySchemaId` is used when:

* `DefaultSchemaInjector` is requested to `forCreateStatement`, `getKeySchema`, `addSchemaFields`
* `CreateSourceProperties` is requested to [getKeyFormatProperties](#getKeyFormatProperties)

## <span id="getValueFormat"> Value Format

```java
Optional<FormatInfo> getValueFormat()
```

`getValueFormat` determines the name of the format based on the value of [FORMAT](#getFormatName) property (if specified) or defaults to [VALUE_FORMAT](CommonCreateConfigs.md#VALUE_FORMAT_PROPERTY) property (from the [PropertiesConfig](#props)).

In the end, `getKeyFormat` creates a `FormatInfo` with the format name and the [value format properties](#getValueFormatProperties) (if specified).

---

`getValueFormat` is used when:

* `DefaultFormatInjector` is requested to `injectForCreateStatement`
* `SourcePropertiesUtil` is requested to [getValueFormat](SourcePropertiesUtil.md#getValueFormat)

### <span id="getValueFormatProperties"> Value Format Properties

```java
Map<String, String> getValueFormatProperties(
  String valueFormat)
```

`getValueFormatProperties` defines the value format properties for various formats (in one go).

Property | Value
---------|------
 `delimiter` | [VALUE_DELIMITER](CommonCreateConfigs.md#VALUE_DELIMITER_PROPERTY) property if specified
 `fullSchemaName` | [VALUE_AVRO_SCHEMA_FULL_NAME](CommonCreateConfigs.md#VALUE_AVRO_SCHEMA_FULL_NAME) or [VALUE_SCHEMA_FULL_NAME](CommonCreateConfigs.md#VALUE_SCHEMA_FULL_NAME) if either is specified
 `schemaId` | [VALUE_SCHEMA_ID](#getValueSchemaId) if specified
 `unwrapPrimitives` | `true` (only if the given `valueFormat` is `PROTOBUF` and the [unwrapProtobufPrimitives](#unwrapProtobufPrimitives) flag is enabled)

### <span id="getValueSchemaId"> VALUE_SCHEMA_ID

```java
Optional<Integer> getValueSchemaId()
```

`getValueSchemaId` is the value of [VALUE_SCHEMA_ID](CommonCreateConfigs.md#VALUE_SCHEMA_ID) property.

---

`getValueSchemaId` is used when:

* `CreateSourceFactory` is requested to [createTableCommand](../CreateSourceFactory.md#createTableCommand)
* `CreateSourceProperties` is requested to [getValueFormatProperties](#getValueFormatProperties)
* `DefaultSchemaInjector` is requested to `forCreateStatement`, `getValueSchema`, `addSchemaFields`
