# KsqlEngineMetrics

## Creating Instance

`KsqlEngineMetrics` takes the following to be created:

* <span id="metricGroupPrefix"> Metrics Group prefix (default: `ksql-engine`)
* <span id="ksqlEngine"> [KsqlEngine](KsqlEngine.md)
* <span id="metricCollectors"> [MetricCollectors](metrics/MetricCollectors.md)
* <span id="customMetricsTags"> Custom Metrics Tags
* <span id="metricsExtension"> [KsqlMetricsExtension](metrics/KsqlMetricsExtension.md)

`KsqlEngineMetrics` is created along with [KsqlEngine](KsqlEngine.md#engineMetrics).

## <span id="getQueryEventListener"> getQueryEventListener

```java
QueryEventListener getQueryEventListener()
```

`getQueryEventListener` creates a [QueryStateMetricsReportingListener](QueryStateMetricsReportingListener.md) with the following:

* [Metrics](#metrics)
* [newCustomMetricsTags](#newCustomMetricsTags)

---

`getQueryEventListener` is used when:

* [KsqlEngine](KsqlEngine.md) is created (to [create an EngineContext](EngineContext.md#queryRegistry))

## <span id="updateMetrics"> updateMetrics

```java
void updateMetrics()
```

`updateMetrics` uses the [MetricCollectors](#metricCollectors) to record the performance metrics:

* [recordMessagesConsumed](#recordMessagesConsumed) with [currentConsumptionRate](metrics/MetricCollectors.md#currentConsumptionRate)
* [recordTotalMessagesConsumed](#recordTotalMessagesConsumed) with [totalMessageConsumption](metrics/MetricCollectors.md#totalMessageConsumption)
* [recordTotalBytesConsumed](#recordTotalBytesConsumed) with [totalBytesConsumption](metrics/MetricCollectors.md#totalBytesConsumption)
* [recordMessagesProduced](#recordMessagesProduced) with [currentProductionRate](metrics/MetricCollectors.md#currentProductionRate)
* [recordMessageConsumptionByQueryStats](#recordMessageConsumptionByQueryStats) with [currentConsumptionRateByQuery](metrics/MetricCollectors.md#currentConsumptionRateByQuery)
* [recordErrorRate](#recordErrorRate) with [currentErrorRate](metrics/MetricCollectors.md#currentErrorRate)

---

`updateMetrics` is used when:

* `KsqlEngine` is [created](KsqlEngine.md#aggregateMetricsCollector)
