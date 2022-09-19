# SerdeFeaturesFactory

## <span id="sanitizeKeyFormat"> sanitizeKeyFormat

```java
KeyFormat sanitizeKeyFormat(
  KeyFormat keyFormat,
  List<SqlType> newKeyColumnSqlTypes,
  boolean allowKeyFormatChangeToSupportNewKeySchema)
```

`sanitizeKeyFormat`...FIXME

---

`sanitizeKeyFormat` is used when:

* `SchemaKGroupedStream` is requested to `aggregate` and `getKeyFormat`
* `SchemaKStream` is requested to [selectKey](SchemaKStream.md#selectKey) and [groupBy](SchemaKStream.md#groupBy)
* `SchemaKTable` is requested to `selectKey` and `groupBy`

### <span id="sanitizeKeyFormatForTypeCompatibility"> sanitizeKeyFormatForTypeCompatibility

```java
KeyFormat sanitizeKeyFormatForTypeCompatibility(
  KeyFormat keyFormat,
  List<SqlType> sqlTypes)
```

`sanitizeKeyFormatForTypeCompatibility` requests the given [KeyFormat](KeyFormat.md) for the [FormatInfo](KeyFormat.md#getFormatInfo) to [create a Format](FormatFactory.md#of).

`sanitizeKeyFormatForTypeCompatibility` returns the [KeyFormat](KeyFormat.md) if every [SqlType](types/SqlType.md) (in the given `sqlTypes` collection) is [supported as a key type](Format.md#supportsKeyType) by the [Format](Format.md).

Otherwise, `sanitizeKeyFormatForTypeCompatibility` [convertToJsonFormat](#convertToJsonFormat) the given [KeyFormat](KeyFormat.md).
