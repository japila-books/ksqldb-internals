# StandaloneExecutorFactory

`StandaloneExecutorFactory` is used to [create a StandaloneExecutor](#create) in [headless](index.md) mode.

## <span id="create"> Creating StandaloneExecutor

```java
StandaloneExecutor create(
  Map<String, Object> properties,
  String queriesFile,
  String installDir,
  Function<KsqlConfig, ServiceContext> serviceContextFactory,
  BiFunction<String, KsqlConfig, ConfigStore> configStoreFactory,
  Function<Supplier<Boolean>, VersionCheckerAgent> versionCheckerFactory,
  StandaloneExecutorConstructor constructor,
  MetricCollectors metricCollectors)
StandaloneExecutor create(
  Map<String, String> properties,
  String queriesFile,
  String installDir,
  MetricCollectors metricCollectors)
```

`create` creates a [KsqlConfig](../KsqlConfig.md) (with the given `properties`) and adds the [addConfluentMetricsContextConfigs](../metrics/MetricCollectors.md#addConfluentMetricsContextConfigs) (from the given [MetricCollectors](../metrics/MetricCollectors.md)).

`create` [creates a ServiceContext](../ServiceContextFactory.md#create).

`create` [configsTopic](../rest/ReservedInternalTopics.md#configsTopic), [KsqlInternalTopicUtils.ensureTopic](../KsqlInternalTopicUtils.md#ensureTopic) and creates a [KafkaConfigStore](KafkaConfigStore.md) (with the config topic name).

`create` creates a [ProcessingLogConfig](../monitoring/ProcessingLogConfig.md).

`create` creates a [ProcessingLogContext](../monitoring/ProcessingLogContext.md#create) (with the [ProcessingLogConfig](../monitoring/ProcessingLogConfig.md), the given [MetricCollectors](../metrics/MetricCollectors.md) and the tags based on [ksql.metrics.tags.custom](../KsqlConfig.md#KSQL_CUSTOM_METRICS_TAGS) configuration property).

`create` creates a `InternalFunctionRegistry`.

`create` creates a [KsqlEngine](../KsqlEngine.md) (with a new [ServiceInfo](../ServiceInfo.md#create), a `SequentialQueryIdGenerator` and no [QueryEventListener](../QueryEventListener.md)s).

`create` [creates a UserFunctionLoader](../UserFunctionLoader.md#newInstance).

`create` creates a [VersionCheckerAgent](../VersionCheckerAgent.md).

In the end, `create` creates a [StandaloneExecutor](StandaloneExecutor.md) (with `failOnNoQueries` flag enabled and [NO_TOPIC_DELETE injectors](../Injectors.md#NO_TOPIC_DELETE)).

---

`create` is used when:

* `KsqlServerMain` is requested to [createExecutable](../rest/KsqlServerMain.md#createExecutable) (with a queries file)
