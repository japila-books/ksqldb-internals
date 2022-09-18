# KeySerdeFactory

`KeySerdeFactory` is an [abstraction](#contract) of [factories of key serdes](#implementations).

## Contract

### <span id="create"> Creating Key Serde

```java
Serde<GenericKey> create(
  FormatInfo format,
  PersistenceSchema schema,
  KsqlConfig ksqlConfig,
  Supplier<SchemaRegistryClient> schemaRegistryClientFactory,
  String loggerNamePrefix,
  ProcessingLogContext processingLogContext,
  Optional<TrackedCallback> tracker)
```

`Serde` (Apache Kafka) of `GenericRow`s

See [GenericKeySerDe](GenericKeySerDe.md#create)

Used when:

* `CreateSourceFactory` is requested to [validateSerdesCanHandleSchemas](CreateSourceFactory.md#validateSerdesCanHandleSchemas)
* `KafkaConsumerFactory` is requested to create a `KafkaConsumer`
* `MaterializationProviderBuilderFactory` is requested to `buildMaterializationProvider`
* `RuntimeBuildContext` is requested to [buildKeySerde](RuntimeBuildContext.md#buildKeySerde)
* `InsertsSubscriber` is requested to `createInsertsSubscriber`
* `InsertValuesExecutor` is requested to `serializeValue`

### <span id="create-Windowed"> Creating Windowed Serde

```java
Serde<Windowed<GenericKey>> create(
  FormatInfo format,
  WindowInfo window,
  PersistenceSchema schema,
  KsqlConfig ksqlConfig,
  Supplier<SchemaRegistryClient> schemaRegistryClientFactory,
  String loggerNamePrefix,
  ProcessingLogContext processingLogContext,
  Optional<TrackedCallback> tracker)
```

`Serde` (Apache Kafka) of `Windowed` (Kafka Streams) of `GenericRow`s

See [GenericKeySerDe](GenericKeySerDe.md#create)

Used when:

* `KafkaConsumerFactory` is requested to create a `KafkaConsumer`
* `RuntimeBuildContext` is requested to [buildKeySerde](RuntimeBuildContext.md#buildKeySerde)

## Implementations

* [GenericKeySerDe](GenericKeySerDe.md)
