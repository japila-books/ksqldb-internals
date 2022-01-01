# KsqlContext

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
