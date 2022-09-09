# Injector

`Injector` is an [abstraction](#contract) of [injectors](#implementations) to [fill in statements](#inject) with more information for execution (e.g., a schema or a kafka topic).

## Contract

### <span id="inject"> Injecting Metadata

```java
<T extends Statement> ConfiguredStatement<T> inject(
  ConfiguredStatement<T> statement)
```

Used when:

* `KsqlContext` is requested to [execute a statement](embedded/KsqlContext.md#execute)
* `StatementExecutor` is requested to [prepare a statement](rest/StatementExecutor.md#prepare)
* `DistributingExecutor` is requested to [execute a statement](rest/DistributingExecutor.md#execute)
* `RequestValidator` is requested to [validate a statement](rest/RequestValidator.md#validate)

## Implementations

* `DefaultFormatInjector`
* [DefaultSchemaInjector](DefaultSchemaInjector.md)
* `InjectorChain`
* [SchemaRegisterInjector](SchemaRegisterInjector.md)
* `SourcePropertyInjector`
* `TopicCreateInjector`
* `TopicDeleteInjector`
