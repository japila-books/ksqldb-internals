# GenericRowSerDe

`GenericRowSerDe` is a [ValueSerdeFactory](ValueSerdeFactory.md).

## Creating Instance

`GenericRowSerDe` takes the following to be created:

* <span id="innerFactory"> Inner [GenericSerdeFactory](GenericSerdeFactory.md)
* <span id="queryId"> Query ID

`GenericRowSerDe` is created when:

* `CreateSourceFactory` is [created](../CreateSourceFactory.md#valueSerdeFactory)
* `InsertsSubscriber` is requested to `createInsertsSubscriber`
* `InsertValuesExecutor` is created
* `KafkaConsumerFactory` is requested to create a `KafkaConsumer`
* `RuntimeBuildContext` utility is used to [create a RuntimeBuildContext](../RuntimeBuildContext.md#of)

## <span id="create"> Creating Value Serde

```java
Serde<GenericRow> create(
  FormatInfo format,
  PersistenceSchema schema,
  KsqlConfig ksqlConfig,
  Supplier<SchemaRegistryClient> srClientFactory,
  String loggerNamePrefix,
  ProcessingLogContext processingLogContext,
  Optional<TrackedCallback> tracker)
```

`create` is part of the [ValueSerdeFactory](ValueSerdeFactory.md#create) abstraction.

---

`create` requests the [inner GenericSerdeFactory](#innerFactory) to [createFormatSerde](GenericSerdeFactory.md#createFormatSerde) (for the `Value` target and `isKey` flag off) and [converts it to a Serde of GenericRows](#toGenericRowSerde).

`create` requests the [inner GenericSerdeFactory](#innerFactory) to [wrapInLoggingSerde](GenericSerdeFactory.md#wrapInLoggingSerde).

`create` requests the `Serde<GenericRow>` to `configure` (with `isKey` flag off).

## <span id="from"> Creating Serde of GenericRows

```java
Serde<GenericRow> from(
  FormatInfo format,
  PersistenceSchema schema,
  KsqlConfig ksqlConfig,
  Supplier<SchemaRegistryClient> schemaRegistryClientFactory,
  String loggerNamePrefix,
  ProcessingLogContext processingLogContext)
```

`from` creates a [GenericRowSerDe](#creating-instance) to [create a Serde of GenericRow](#create).
