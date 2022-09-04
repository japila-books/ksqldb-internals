# MetaStoreImpl

`MetaStoreImpl` is a [MutableMetaStore](MutableMetaStore.md).

## Creating Instance

`MetaStoreImpl` takes the following to be created:

* <span id="dataSources"> Data Sources (`Map<SourceName, SourceInfo>`)
* <span id="functionRegistry"> [FunctionRegistry](FunctionRegistry.md)
* <span id="typeRegistry"> [TypeRegistry](TypeRegistry.md)
* <span id="dropConstraints"> Drop constraints (`Map<SourceName, Set<SourceName>>`)

`MetaStoreImpl` is created when:

* `EngineExecutor` is requested to [sourceTablePlan](EngineExecutor.md#sourceTablePlan)
* `KsqlEngine` is [created](KsqlEngine.md#metaStore)

## <span id="putSource"> Registering DataSource

```java
void putSource(
  DataSource dataSource,
  boolean allowReplace)
```

`putSource` is part of the [MutableMetaStore](MutableMetaStore.md#putSource) abstraction.

---

`putSource` adds the given [DataSource](DataSource.md) to the [dataSources](#dataSources) registry.

`putSource` prints out the following INFO message to the logs:

```text
Source [name] created on the metastore
```

`putSource` re-builds DROP constraints (if there are any).

## Logging

Enable `ALL` logging level for `io.confluent.ksql.metastore.MetaStoreImpl` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.io.confluent.ksql.metastore.MetaStoreImpl=ALL
```

Refer to [Logging](logging.md).
