# PersistentQueryMetadata

`PersistentQueryMetadata` is an [extension](#contract) of the [QueryMetadata](QueryMetadata.md) abstraction for [metadata of persistent queries](#implementations).

## Contract (Subset)

### <span id="getPersistentQueryType"> getPersistentQueryType

```java
KsqlConstants.PersistentQueryType getPersistentQueryType()
```

One of the following:

* `CREATE_SOURCE`
* `CREATE_AS`
* `INSERT`

Used when:

* `QueryRegistryImpl` is requested to [registerPersistentQuery](QueryRegistryImpl.md#registerPersistentQuery) and [unregisterQuery](QueryRegistryImpl.md#unregisterQuery)
* `PersistentQueryMetadataImpl` is [created](PersistentQueryMetadataImpl.md#persistentQueryType)
* `ValidatedCommandFactory` is requested to [createForTerminateQuery](rest/ValidatedCommandFactory.md#createForTerminateQuery)

### <span id="getProcessingLogger"> getProcessingLogger

```java
ProcessingLogger getProcessingLogger()
```

`ProcessingLogger` of this persistent query

## Implementations

* `BinPackedPersistentQueryMetadataImpl`
* [PersistentQueryMetadataImpl](PersistentQueryMetadataImpl.md)
