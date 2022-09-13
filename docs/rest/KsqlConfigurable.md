# KsqlConfigurable

`KsqlConfigurable` is an [abstraction](#contract) of [configurable services](#implementations) (of a [ksqlDB API server](KsqlRestApplication.md#configurables) instance).

## Contract

### <span id="configure"> Configuring Service

```java
void configure(
  KsqlConfig config)
```

Configures this service with the given [KsqlConfig](../KsqlConfig.md)

Used when:

* `KsqlRestApplication` is requested to [startAsync](KsqlRestApplication.md#startAsync)

## Implementations

* [InteractiveStatementExecutor](InteractiveStatementExecutor.md)
* [KsqlResource](KsqlResource.md)
* [StreamedQueryResource](StreamedQueryResource.md)
