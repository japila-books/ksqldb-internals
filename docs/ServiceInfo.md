# ServiceInfo

## Creating Instance

`ServiceInfo` takes the following to be created:

* <span id="serviceId"> [ksql.service.id](KsqlConfig.md#KSQL_SERVICE_ID_CONFIG)
* <span id="customMetricsTags"> Custom Metrics Tags (`Map<String, String>`)
* <span id="metricsExtension"> `KsqlMetricsExtension`
* <span id="metricsPrefix"> Metrics Prefix

`ServiceInfo` is created using [create](#create) factory.

## <span id="create"> Creating ServiceInfo

```java
ServiceInfo create(
  KsqlConfig ksqlConfig)
ServiceInfo create(
  KsqlConfig ksqlConfig,
  String metricsPrefix)
```

`create`...FIXME

---

`create` is used when:

* `KsqlRestApplication` is requested to [build a KsqlRestApplication](rest/KsqlRestApplication.md#buildApplication)
* `KsqlContext` is [created](embedded/KsqlContext.md#create)
* `StandaloneExecutorFactory` is [created](headless/StandaloneExecutorFactory.md#create)
