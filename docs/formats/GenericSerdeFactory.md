# GenericSerdeFactory

## Creating Instance

`GenericSerdeFactory` takes the following to be created:

* <span id="formatFactory"> [Format](Format.md) factory ([FormatFactory.of](FormatFactory.md#of))

`GenericSerdeFactory` is created along with the following:

* [GenericKeySerDe](GenericKeySerDe.md#innerFactory)
* [GenericRowSerDe](GenericRowSerDe.md#innerFactory)

## <span id="createFormatSerde"> Creating Format Serde

```java
Serde<List<?>> createFormatSerde(
  String target,
  FormatInfo formatInfo,
  PersistenceSchema schema,
  KsqlConfig ksqlConfig,
  Supplier<SchemaRegistryClient> schemaRegistryClientFactory,
  boolean isKey)
```

`createFormatSerde` creates a [Format](Format.md) (using the [factory](#formatFactory) for the given `FormatInfo`) to [get a Serde](Format.md#getSerde) for the given `PersistenceSchema`.

In case of any exception, `createFormatSerde` throws a `SchemaNotSupportedException`:

```text
[target] format does not support schema.
format: [name]
schema: [schema]
reason: [reason]
```

---

`createFormatSerde` is used when:

* `GenericKeySerDe` is requested to [createInner](GenericKeySerDe.md#createInner)
* `GenericRowSerDe` is requested to [create a Serde](GenericRowSerDe.md#create)
