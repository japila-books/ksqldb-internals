# EngineContext

## <span id="parse"> parse

```java
List<ParsedStatement> parse(
  String sql)
```

`parse` requests the [KsqlParser](#parser) to [parse the SQL text](KsqlParser.md#parse).

`parse` is used when:

* `EngineContext` is requested to [substituteVariables](#substituteVariables)
* `KsqlEngine` is requested to [parse a SQL text](KsqlEngine.md#parse)
* `SandboxedExecutionContext` is requested to `parse`

## <span id="prepare"> prepare

```java
PreparedStatement<?> prepare(
  ParsedStatement stmt,
  Map<String, String> variablesMap)
```

`prepare` requests the [KsqlParser](#parser) to [prepare the ParsedStatement](KsqlParser.md#parse) and creates a new `PreparedStatement`.

`prepare` is used when:

* `KsqlEngine` is requested to [prepare a ParsedStatement](KsqlEngine.md#prepare)
* `SandboxedExecutionContext` is requested to `prepare` a `ParsedStatement`

## <span id="create"> Creating EngineContext

```java
EngineContext create(
  ServiceContext serviceContext,
  ProcessingLogContext processingLogContext,
  MutableMetaStore metaStore,
  QueryIdGenerator queryIdGenerator,
  QueryCleanupService cleanupService,
  KsqlConfig ksqlConfig,
  Collection<QueryEventListener> registrationListeners)
```

`create`...FIXME

`create` is used when:

* `KsqlEngine` is [created](KsqlEngine.md#primaryContext)
