# ConfigStore

`ConfigStore` is an [abstraction](#contract) of [config stores](#implementations) (with [KsqlConfig](#getKsqlConfig)) for [StandaloneExecutorFactory](StandaloneExecutorFactory.md) to [create a StandaloneExecutor](StandaloneExecutorFactory.md#create).

## Contract

### <span id="getKsqlConfig"> getKsqlConfig

```java
KsqlConfig getKsqlConfig()
```

[KsqlConfig](../KsqlConfig.md)

See [KafkaConfigStore](KafkaConfigStore.md#getKsqlConfig)

Used when:

* `StandaloneExecutorFactory` is requested to [create a StandaloneExecutor](StandaloneExecutorFactory.md#create)

## Implementations

* [KafkaConfigStore](KafkaConfigStore.md)
