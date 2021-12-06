# KsqlEngine

## Creating Instance

`KsqlEngine` takes the following to be created:

* <span id="serviceContext"> ServiceContext
* <span id="processingLogContext"> ProcessingLogContext
* <span id="serviceId"> Service ID
* <span id="metaStore"> MutableMetaStore
* <span id="engineMetricsFactory"> `Function<KsqlEngine, KsqlEngineMetrics>`
* <span id="queryIdGenerator"> QueryIdGenerator
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="queryEventListeners"> `QueryEventListener`s

`KsqlEngine` is created when:

* `KsqlContext` is requested to [create](embedded/KsqlContext.md#create)
* `KsqlRestApplication` is requested to [buildApplication](rest/KsqlRestApplication.md#buildApplication)
* `StandaloneExecutorFactory` is requested to [create](rest/StandaloneExecutorFactory.md#create)

## <span id="primaryContext"> EngineContext

`KsqlEngine` [creates an EngineContext](EngineContext.md#create) when [created](#creating-instance).

The `EngineContext` is used when:

* [getAllLiveQueries](#getAllLiveQueries)
* _many others_

## <span id="getAllLiveQueries"> getAllLiveQueries

```java
List<QueryMetadata> getAllLiveQueries()
```

`getAllLiveQueries`...FIXME

`getAllLiveQueries` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#getAllLiveQueries) abstraction.

## <span id="parse"> Parsing SQL Text

```java
List<ParsedStatement> parse(
  String sql)
```

`parse`...FIXME

`parse` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#parse) abstraction.

## <span id="prepare"> Preparing ParsedStatement

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
```

`prepare` requests the [EngineContext](#primaryContext) to [prepare the given ParsedStatement](EngineContext.md#prepare).

`prepare` is part of the [KsqlExecutionContext](KsqlExecutionContext.md#prepare) abstraction.
