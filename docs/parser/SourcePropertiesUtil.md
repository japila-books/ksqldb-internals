# SourcePropertiesUtil

## <span id="getValueFormat"> getValueFormat

```java
FormatInfo getValueFormat(
  CreateSourceProperties properties)
```

`getValueFormat` takes the [value_format](#getValueFormat) property from the given [CreateSourceProperties](CreateSourceProperties.md) or throws an `IllegalStateException`:

```text
Value format not present
```

`getValueFormat` is used when:

* `CreateSourceFactory` is requested to [buildFormats](../CreateSourceFactory.md#buildFormats)
* `DefaultSchemaInjector` is requested to `getValueSchema`
* `SchemaRegisterInjector` is requested to `registerForCreateSource`
