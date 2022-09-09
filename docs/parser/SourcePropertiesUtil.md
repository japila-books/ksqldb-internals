# SourcePropertiesUtil

## <span id="getKeyFormat"> getKeyFormat

```java
FormatInfo getKeyFormat(
  CreateSourceProperties properties,
  SourceName sourceName)
```

`getKeyFormat` requests the given [CreateSourceProperties](CreateSourceProperties.md) for [getKeyFormat](CreateSourceProperties.md#getKeyFormat) of the `SourceName` (if available) or throws an `IllegalStateException`:

```text
Key format not present
```

---

`getKeyFormat` is used when:

* `CreateSourceFactory` is requested to [buildFormats](../CreateSourceFactory.md#buildFormats)
* `DefaultSchemaInjector` is requested to `getKeySchema`, `addSchemaFields`
* `SchemaRegisterInjector` is requested to [registerForCreateSource](../SchemaRegisterInjector.md#registerForCreateSource)

## <span id="getValueFormat"> getValueFormat

```java
FormatInfo getValueFormat(
  CreateSourceProperties properties)
```

`getValueFormat` requests the given [CreateSourceProperties](CreateSourceProperties.md) for [getValueFormat](CreateSourceProperties.md#getValueFormat) (if available) or throws an `IllegalStateException`:

```text
Value format not present
```

---

`getValueFormat` is used when:

* `CreateSourceFactory` is requested to [buildFormats](../CreateSourceFactory.md#buildFormats)
* `DefaultSchemaInjector` is requested to `getValueSchema`, `addSchemaFields`
* `SchemaRegisterInjector` is requested to [registerForCreateSource](../SchemaRegisterInjector.md#registerForCreateSource)
