# KsqlSchemaRegistryClientFactory

## Creating Instance

`KsqlSchemaRegistryClientFactory` takes the following to be created:

* <span id="config"> [KsqlConfig](KsqlConfig.md)
* <span id="serviceSupplier"> `RestService` factory
* <span id="sslContext"> `SSLContext`
* <span id="schemaRegistryClientFactory"> `SchemaRegistryClientFactory`
* <span id="httpHeaders"> HTTP Headers (`Map<String, String>`)

`KsqlSchemaRegistryClientFactory` is created when:

* `ServiceContextFactory` is requested for a [ServiceContext](ServiceContextFactory.md#create)
* `KsqlRestApplication` is requested to [build a KsqlRestApplication](rest/KsqlRestApplication.md#buildApplication)
* `PreconditionChecker` is requested to `buildServiceContext`

## <span id="ksql.schema.registry.url"> ksql.schema.registry.url

`KsqlSchemaRegistryClientFactory` uses [ksql.schema.registry.url](KsqlConfig.md#SCHEMA_REGISTRY_URL_PROPERTY).
