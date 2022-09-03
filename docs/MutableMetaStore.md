# MutableMetaStore

`MutableMetaStore` is an [extension](#contract) of the [MetaStore](MetaStore.md) abstraction for [mutable metastores](#implementations).

## Contract

### <span id="addSourceReferences"> addSourceReferences

```java
void addSourceReferences(
  SourceName sourceName,
  Set<SourceName> sourceReferences)
```

### <span id="copy"> copy

```java
MutableMetaStore copy()
```

Makes a copy of this `MutableMetaStore`

Used when:

* `EngineContext` is requested to [createSandbox](EngineContext.md#createSandbox)

### <span id="deleteSource"> deleteSource

```java
void deleteSource(
  SourceName sourceName) // (1)!
void deleteSource(
  SourceName sourceName,
  boolean restoreInProgress)
```

1. Turns `restoreInProgress` off (`false`)

### <span id="putSource"> Registering DataSource

```java
void putSource(
  DataSource dataSource,
  boolean allowReplace)
```

See [MetaStoreImpl](MetaStoreImpl.md#putSource)

Used when:

* `DdlCommandExec.Executor` is requested to [execute a CreateStreamCommand](DdlCommandExec.Executor.md#executeCreateStream), [executeCreateTable](DdlCommandExec.Executor.md#executeCreateTable), [executeAlterSource](DdlCommandExec.Executor.md#executeAlterSource)
* `EngineExecutor` is requested to [sourceTablePlan](EngineExecutor.md#sourceTablePlan)

## Implementations

* [MetaStoreImpl](MetaStoreImpl.md)
