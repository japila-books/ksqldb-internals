# ValueSerdeFactory

`ValueSerdeFactory` is an [abstraction](#contract) of [factories of value serdes](#implementations).

## Contract

### <span id="create"> Creating Value Serde

```java
Serde<GenericRow> create(
  FormatInfo format,
  PersistenceSchema schema,
  KsqlConfig ksqlConfig,
  Supplier<SchemaRegistryClient> schemaRegistryClientFactory,
  String loggerNamePrefix,
  ProcessingLogContext processingLogContext,
  Optional<TrackedCallback> tracker)
```

`Serde` (Apache Kafka) of `GenericRow`s

See [GenericRowSerDe](GenericRowSerDe.md#create)

Used when:

* `CreateSourceFactory` is requested to [validateSerdesCanHandleSchemas](../CreateSourceFactory.md#validateSerdesCanHandleSchemas)
* `KafkaConsumerFactory` is requested to create a `KafkaConsumer`
* `RuntimeBuildContext` is requested to [build a value Serde](../RuntimeBuildContext.md#buildValueSerde)
* `InsertsSubscriber` is requested to `createInsertsSubscriber`
* `InsertValuesExecutor` is requested to `serializeValue`
* `GenericRowSerDe` is requested to [create a Serde](GenericRowSerDe.md#from)

## Implementations

* [GenericRowSerDe](GenericRowSerDe.md)
