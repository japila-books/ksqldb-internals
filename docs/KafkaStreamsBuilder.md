# KafkaStreamsBuilder

`KafkaStreamsBuilder` is an [abstraction](#contract) of [KafkaStreams builders](#implementations).

## Contract

### <span id="build"> Building KafkaStreams Client

```java
KafkaStreams build(
  Topology topology,
  Map<String, Object> conf)
```

[KafkaStreamsBuilderImpl](KafkaStreamsBuilderImpl.md#build)

Used when:

* `QueryMetadataImpl` is requested to [initialize](QueryMetadataImpl.md#initialize)

### <span id="buildNamedTopologyWrapper"> buildNamedTopologyWrapper

```java
KafkaStreamsNamedTopologyWrapper buildNamedTopologyWrapper(
  Map<String, Object> conf)
```

[KafkaStreamsBuilderImpl](KafkaStreamsBuilderImpl.md#buildNamedTopologyWrapper)

Used when:

* `SharedKafkaStreamsRuntime` is [created](SharedKafkaStreamsRuntime.md#kafkaStreams)
* `SharedKafkaStreamsRuntimeImpl` is requested to [restartStreamsRuntime](SharedKafkaStreamsRuntimeImpl.md#restartStreamsRuntime)

## Implementations

* [KafkaStreamsBuilderImpl](KafkaStreamsBuilderImpl.md)
