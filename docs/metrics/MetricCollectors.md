# MetricCollectors

`MetricCollectors` allows aggregating metrics across [registered MetricCollectors](#collectorMap).

## Creating Instance

`MetricCollectors` takes the following to be created:

* [Metrics](#metrics)

`MetricCollectors` is created when:

* `KsqlContext` is [created](../embedded/KsqlContext.md#create)
* `KsqlServerMain` standalone application is [launched](../rest/KsqlServerMain.md#main) (and [creates an Executable](../rest/KsqlServerMain.md#createExecutable))

### <span id="metrics"><span id="getMetrics"> Metrics

`MetricCollectors` can be given a `Metrics` ([Apache Kafka]({{ book.kafka }}/Metrics)) when [created](#creating-instance).

This `Metrics` is configured to use `JmxReporter` with `io.confluent.ksql.metrics` JMX namespace.

`Metrics` is used to register new metric reporters in [addConfigurableReporter](#addConfigurableReporter).

#### getMetrics

```java
Metrics getMetrics()
```

`getMetrics` returns the [Metrics](#metrics).

---

`getMetrics` is used when:

* `ConsumerCollector` is requested to [configure](ConsumerCollector.md#configure)
* `ProducerCollector` is requested to [configure](ProducerCollector.md#configure)
* `StreamsErrorCollector` is [created](StreamsErrorCollector.md#metrics)
* `KsqlContext` utility is used to [create a KsqlContext](../embedded/KsqlContext.md#create)
* `KsqlEngineMetrics` is [created](../KsqlEngineMetrics.md#metrics)
* `QueryBuilder` is requested to [buildStreamsProperties](../QueryBuilder.md#buildStreamsProperties)
* `KsqlRestApplication` utility is used to [build a KsqlRestApplication](../rest/KsqlRestApplication.md#buildApplication)
* `StandaloneExecutorFactory` is requested to [create a StandaloneExecutor](../headless/StandaloneExecutorFactory.md#create)

## <span id="collectorMap"> MetricCollectors by ID

```java
Map<String, MetricCollector> collectorMap
```

`MetricCollectors` creates an empty `collectorMap` registry of [MetricCollector](MetricCollector.md)s by id when [created](#creating-instance).

A new [MetricCollector](MetricCollector.md) is added to the `collectorMap` registry in [addCollector](#addCollector) and removed in [remove](#remove).

The [MetricCollector](MetricCollector.md)s themselves in `collectorMap` are used when:

* [getStatsFor](#getStatsFor)
* [currentConsumptionRateByQuery](#currentConsumptionRateByQuery)
* [aggregateStat](#aggregateStat)
* [currentErrorRate](#currentErrorRate)

## <span id="addCollector"> Registering MetricCollector

```java
String addCollector(
  String id,
  MetricCollector collector)
```

`addCollector`...FIXME

---

`addCollector` is used when:

* `ConsumerCollector` is requested to [configure](#configure)
* `ProducerCollector` is requested to [configure](ProducerCollector.md#configure)
* `StreamsErrorCollector` is used to [create a StreamsErrorCollector](StreamsErrorCollector.md#create)

## <span id="addConfigurableReporter"> addConfigurableReporter

```java
void addConfigurableReporter(
  KsqlConfig ksqlConfig)
```

`addConfigurableReporter`...FIXME

---

`addConfigurableReporter` is used when:

* `KsqlRestApplication` is [created](../rest/KsqlRestApplication.md)
* `StandaloneExecutor` is [created](../headless/StandaloneExecutor.md)

## <span id="aggregateStat"> aggregateStat

```java
double aggregateStat(
  String name,
  boolean isError)
```

`aggregateStat` sums up [aggregateStat](MetricCollector.md#aggregateStat)s of all the [registered MetricCollectors](#collectorMap).

---

`aggregateStat` is used when:

* `MetricCollectors` is requested for [currentProductionRate](#currentProductionRate), [currentConsumptionRate](#currentConsumptionRate), [totalMessageConsumption](#totalMessageConsumption), [totalBytesConsumption](#totalBytesConsumption)

## <span id="currentConsumptionRateByQuery"> currentConsumptionRateByQuery

```java
Collection<Double> currentConsumptionRateByQuery()
```

`currentConsumptionRateByQuery`...FIXME

---

`currentConsumptionRateByQuery` is used when:

* `KsqlEngineMetrics` is requested to [updateMetrics](../KsqlEngineMetrics.md#updateMetrics)

## <span id="currentErrorRate"> currentErrorRate

```java
double currentErrorRate()
```

`currentErrorRate`...FIXME

---

`currentErrorRate` is used when:

* `KsqlEngineMetrics` is requested to [updateMetrics](../KsqlEngineMetrics.md#updateMetrics)

## <span id="getStatsFor"> getStatsFor

```java
Collection<TopicSensors.Stat> getStatsFor(
  String topic,
  boolean isError)
```

`getStatsFor`...FIXME

---

`getStatsFor` is used when:

* `MetricCollectors` is requested to [getAndFormatStatsFor](#getAndFormatStatsFor)
* `SourceDescriptionFactory` is requested for a `SourceDescription`
