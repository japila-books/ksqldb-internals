# KsqlContext

## Creating Instance

`KsqlContext` takes the following to be created:

* <span id="serviceContext"> ServiceContext
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* <span id="injectorFactory"> `BiFunction<KsqlExecutionContext, ServiceContext, Injector>`

`KsqlContext` is created when:

* `KsqlContext` utility is used to [create a KsqlContext](#create)

## <span id="create"> Creating KsqlContext

```java
KsqlContext create(
  KsqlConfig ksqlConfig,
  ProcessingLogContext processingLogContext)
```

`create`...FIXME

`create` is used when:

* [EmbeddedKsql](EmbeddedKsql.md) standalone application is launched
