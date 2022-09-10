# KsqlContext

`KsqlContext` is part of [ksqlDB embedded mode](index.md) for developers to use the [ksqlDB engine](#ksqlEngine) embedded in their applications to [execute ksql statements](#sql).

## Creating Instance

`KsqlContext` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](../ServiceContext.md)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* [KsqlEngine](#ksqlEngine)
* <span id="injectorFactory"> [DEFAULT injectors](../Injectors.md#DEFAULT)

`KsqlContext` is created using [create](#create) utility.

### <span id="ksqlEngine"> KsqlEngine

`KsqlContext` is given a [KsqlEngine](../KsqlEngine.md) when [created](#creating-instance).

The `KsqlEngine` is active until [close](#close).

The `KsqlEngine` is used when:

* [getMetaStore](#getMetaStore)
* [sql](#sql)
* [getRunningQueries](#getRunningQueries) and [getPersistentQueries](#getPersistentQueries)
* [terminateQuery](#terminateQuery)

## <span id="create"> Creating KsqlContext

```java
KsqlContext create(
  KsqlConfig ksqlConfig,
  ProcessingLogContext processingLogContext)
```

`create` [creates a ServiceContext](../ServiceContextFactory.md#create) (with the given [KsqlConfig](../KsqlConfig.md)).

`create` [creates a UserFunctionLoader](../UserFunctionLoader.md#newInstance) to [load UDFs](../UserFunctionLoader.md#load).

`create` [creates a ServiceInfo](../ServiceInfo.md#create).

`create` creates a [KsqlEngine](../KsqlEngine.md) (with a new `SequentialQueryIdGenerator`).

In the end, `create` creates a [KsqlContext](#creating-instance) (with the [DEFAULT injectors](../Injectors.md#DEFAULT)).

## <span id="sql"> Executing SQL Text

```java
List<QueryMetadata> sql(
  String sql) // (1)!
List<QueryMetadata> sql(
  String sql,
  Map<String, ?> overriddenProperties)
```

1. Uses an empty `overriddenProperties` collection

`sql` requests the [KsqlEngine](#ksqlEngine) to [parse](../KsqlEngine.md#parse) the sql (into a collection of prepared statements).

`sql` requests the [KsqlEngine](#ksqlEngine) to [create a sandbox](../KsqlEngine.md#createSandbox) to [execute](#execute) the statements within (one by one).

`sql` [executes](#execute) the (prepared) statements (one by one).

`sql` requests [persistent queries](../PersistentQueryMetadata.md) to [start](../QueryMetadata.md#start) (if there are any).

`sql` prints out the following WARN message to the logs for all other non-persistent [queries](../QueryMetadata.md):

```text
Ignoring statement: [sql]
Only CREATE statements can run in KSQL embedded mode.
```

In the end, `sql` returns all [queries](../QueryMetadata.md)s (persistent and non-persistent).

### <span id="execute"> Executing Statement

```java
ExecuteResult execute(
  KsqlExecutionContext executionContext,
  ParsedStatement stmt,
  KsqlConfig ksqlConfig,
  Map<String, Object> mutableSessionPropertyOverrides,
  Injector injector)
```

`execute`...FIXME

## Demo

```scala
import io.confluent.ksql.util.KsqlConfig
import scala.jdk.CollectionConverters._
val props = Map("bootstrap.servers" -> ":9092").asJava
val ksqlConfig = new KsqlConfig(props)

import io.confluent.ksql.logging.processing.ProcessingLogContext
val processingLogContext = ProcessingLogContext.create()

import io.confluent.ksql.metrics.MetricCollectors
val metricCollectors = new MetricCollectors()

import io.confluent.ksql.embedded.KsqlContext
val ksqlContext = KsqlContext.create(
  ksqlConfig,
  processingLogContext,
  metricCollectors)
```

## Logging

Enable `ALL` logging level for `io.confluent.ksql.embedded.KsqlContext` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.io.confluent.ksql.embedded.KsqlContext=ALL
```

Refer to [Logging](../logging.md).
