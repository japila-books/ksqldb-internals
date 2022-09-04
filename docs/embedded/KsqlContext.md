# KsqlContext

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

## Creating Instance

`KsqlContext` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](../ServiceContext.md)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* <span id="injectorFactory"> [Injector](../Injector.md) Factory (`BiFunction<KsqlExecutionContext, ServiceContext, Injector>`)

`KsqlContext` is created using [create](#create) utility.

### <span id="create"> Creating KsqlContext

```java
KsqlContext create(
  KsqlConfig ksqlConfig,
  ProcessingLogContext processingLogContext)
```

`create`...FIXME

## <span id="sql"> Executing SQL Text

```java
List<QueryMetadata> sql(
  String sql)
List<QueryMetadata> sql(
  String sql,
  Map<String, ?> overriddenProperties)
```

`sql`...FIXME

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
