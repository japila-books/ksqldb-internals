# MetricCollector

`MetricCollector` is an [abstraction](#contract) of [metrics collectors](#implementations).

## Contract

### <span id="aggregateStat"> aggregateStat

```java
double aggregateStat(
  String name,
  boolean isError)
```

Used when:

* `MetricCollectors` is requested to [currentConsumptionRateByQuery](MetricCollectors.md#currentConsumptionRateByQuery) and [aggregateStat](MetricCollectors.md#aggregateStat)

### <span id="stats"> stats

```java
Collection<TopicSensors.Stat> stats(
  String topic,
  boolean isError)
```

Used when:

* `MetricCollectors` is requested to [getStatsFor](MetricCollectors.md#getStatsFor)

## Implementations

* `ConsumerCollector`
* `ProducerCollector`
* [StreamsErrorCollector](StreamsErrorCollector.md)

## <span id="errorRate"> errorRate

```java
double errorRate()
```

`errorRate` is `0.0` by default.

---

`errorRate` is used when:

* `MetricCollectors` is requested to [currentErrorRate](MetricCollectors.md#currentErrorRate)

## <span id="getGroupId"> getGroupId

```java
String getGroupId()
```

`getGroupId` is `null` (undefined) by default.

---

`getGroupId` is used when:

* `MetricCollectors` is requested to [currentConsumptionRateByQuery](MetricCollectors.md#currentConsumptionRateByQuery)
