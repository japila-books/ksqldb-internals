# ServiceContext

`ServiceContext` is an [abstraction](#contract) of [service contexts](#implementations).

## Contract (Subset)

### <span id="getAdminClient"> getAdminClient

```java
Admin getAdminClient()
```

`Admin` ([Apache Kafka]({{ book.kafka }}/clients/admin/Admin))

Used when:

* `KsqlEngine` is requested to [createStreamPullQuery](KsqlEngine.md#createStreamPullQuery)
* `KsqlAuthorizationValidatorFactory` is requested to `isTopicAccessValidatorEnabled`
* `KafkaClusterUtil` is requested to `getKafkaClusterId`
* `SandboxedServiceContext` is created
* `ListTopicsExecutor` is requested to `execute` (a `LIST TOPICS` statement)

### <span id="getConsumerGroupClient"> getConsumerGroupClient

```java
KafkaConsumerGroupClient getConsumerGroupClient()
```

Used when:

* `QueryCleanupTask` is requested to `run`
* `ScalablePushRegistry` is requested to `deleteConsumerGroup`
* `SandboxedServiceContext` is created
* `ListSourceExecutor` is requested to `queryOffsetSummaries`

### <span id="getKsqlClient"> getKsqlClient

```java
SimpleKsqlClient getKsqlClient()
```

Used when:

* `HARouting` is requested to [forwardTo](HARouting.md#forwardTo)
* `PushRouting` is requested to [forwardTo](PushRouting.md#forwardTo)
* `SendHeartbeatService` is requested to `runOneIteration`
* `SendLagService` is requested to `runOneIteration`
* `ListQueriesExecutor` is requested to `execute` (a `LIST QUERIES` statement)
* `ListSourceExecutor` is requested to `sourceDescriptionList`
* `TerminateQueryExecutor` is requested to `execute` (a `TERMINATE QUERY` statement)

## Implementations

* `DefaultServiceContext`
* `LazyServiceContext`
* `SandboxedServiceContext`
