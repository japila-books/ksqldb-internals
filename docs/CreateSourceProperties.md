# CreateSourceProperties

## Creating Instance

`CreateSourceProperties` takes the following to be created:

* <span id="originals"> Originals (`Map<String, Literal>`)
* <span id="durationParser"> Duration Parser (`Function<String, Duration>`)

`CreateSourceProperties` is created using [from](#from) factory and when:

* [withKeyValueSchemaName](#withKeyValueSchemaName)
* [withPartitions](#withPartitions)
* [withFormats](#withFormats)

While being created, `CreateSourceProperties` creates a `PropertiesConfig` and...FIXME

## <span id="from"> Creating CreateSourceProperties

```java
CreateSourceProperties from(
  Map<String, Literal> literals)
```

`from` creates a [CreateSourceProperties](#creating-instance) (with the given `literals` and the default `DurationParser`).

`from` is used when:

* `AstBuilder.Visitor` is requested to [visitCreateTable](AstBuilder.md#visitCreateTable), [visitCreateStream](AstBuilder.md#visitCreateStream), [visitAssertStream](AstBuilder.md#visitAssertStream) and [visitAssertTable](AstBuilder.md#visitAssertTable)

## <span id="getValueFormat"> value_format

```java
Optional<FormatInfo> getValueFormat()
```

`getValueFormat` takes [FORMAT](#getFormatName) (if defined) or defaults to [VALUE_FORMAT](CommonCreateConfigs.md#VALUE_FORMAT_PROPERTY) property (using the [PropertiesConfig](#props)).

If defined (using either configuration property), the value format is converted to `FormatInfo` (with [getValueFormatProperties](#getValueFormatProperties)).

`getValueFormat` is used when:

* `DefaultFormatInjector` is requested to `injectForCreateStatement`
* `SourcePropertiesUtil` is requested to [getValueFormat](SourcePropertiesUtil.md#getValueFormat)
