# QueryMetadata

`QueryMetadata` is an [abstraction](#contract) of [query metadatas](#implementations).

## Contract (Subset)

### <span id="getKafkaStreams"> getKafkaStreams

```java
KafkaStreams getKafkaStreams()
```

`KafkaStreams` ([Kafka Streams]({{ book.kafka_streams }}/KafkaStreams)) to execute this query

Used when:

* `SandboxedExecutionContext` is requested to [execute](SandboxedExecutionContext.md#execute)
* `PersistentQueryMetadataImpl` is requested to [initialize](PersistentQueryMetadataImpl.md#initialize)
* `TransientQueryMetadata` is requested to [isRunning](TransientQueryMetadata.md#isRunning)
* `PersistentQuerySaturationMetrics` is requested to `measure`
* `QueryMetricsUtil` is requested to `initializePullStreamMetricsCallback`

### <span id="getQueryType"> getQueryType

```java
KsqlConstants.KsqlQueryType getQueryType()
```

[KsqlQueryType](KsqlQueryType.md) of this query

Used when:

* `QueryDescriptionFactory` is requested to `create` a `QueryDescription`
* `ListQueriesExecutor` is requested to `getLocalSimple` (for `LIST QUERIES` command)

### <span id="getTopology"> getTopology

```java
Topology getTopology()
```

`Topology` ([Kafka Streams]({{ book.kafka_streams }}/Topology)) of this query

Used when:

* `PersistentQueryMetadataImpl` is requested to [initialize](PersistentQueryMetadataImpl.md#initialize)
* `SandboxedSharedKafkaStreamsRuntimeImpl` is created
* `SharedKafkaStreamsRuntimeImpl` is requested to [start a query](SharedKafkaStreamsRuntimeImpl.md#start) and [restartStreamsRuntime](SharedKafkaStreamsRuntimeImpl.md#restartStreamsRuntime)

### <span id="start"> start

```java
void start()
```

Starts this query

Used when:

* `KsqlContext` is requested to [sql](embedded/KsqlContext.md#sql)
* `StandaloneExecutor` is requested to [processesQueryFile](headless/StandaloneExecutor.md#processesQueryFile)
* `CommandRunner` is requested to [processPriorCommands](rest/CommandRunner.md#processPriorCommands)
* `InteractiveStatementExecutor` is requested to [executePlan](rest/InteractiveStatementExecutor.md#executePlan)

## Implementations

* [PersistentQueryMetadata](PersistentQueryMetadata.md)
* [QueryMetadataImpl](QueryMetadataImpl.md)
