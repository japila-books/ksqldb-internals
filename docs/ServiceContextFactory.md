# ServiceContextFactory

## <span id="create"> Creating ServiceContext

```java
ServiceContext create(
  KsqlConfig ksqlConfig,
  Supplier<SimpleKsqlClient> ksqlClientSupplier)
ServiceContext create(
  KsqlConfig ksqlConfig,
  KafkaClientSupplier kafkaClientSupplier,
  Supplier<SchemaRegistryClient> srClientFactory,
  Supplier<ConnectClient> connectClientSupplier,
  Supplier<SimpleKsqlClient> ksqlClientSupplier)
```

`create` creates a `DefaultServiceContext` with the following:

* `DefaultKafkaClientSupplier`
* [KsqlSchemaRegistryClientFactory](KsqlSchemaRegistryClientFactory.md)

---

`create` is used when:

* `KsqlContext` utility is requested for a [KsqlContext](embedded/KsqlContext.md#create)
* `RestServiceContextFactory` is requested to [create a ServiceContext](rest/RestServiceContextFactory.md#create)
* `StandaloneExecutorFactory` utility is requested for a [StandaloneExecutor](headless/StandaloneExecutorFactory.md#create)
