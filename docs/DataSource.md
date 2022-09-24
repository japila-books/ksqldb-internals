# DataSource

`DataSource` is an [abstraction](#contract) of [data sources](#implementations).

## Contract (Subset)

### <span id="getDataSourceType"> getDataSourceType

```java
DataSourceType getDataSourceType()
```

[DataSourceType](#DataSourceType) of this `DataSource`

### <span id="getSchema"> getSchema

```java
LogicalSchema getSchema()
```

[LogicalSchema](LogicalSchema.md) of this `DataSource`

See [StructuredDataSource](StructuredDataSource.md#getSchema)

### <span id="getTimestampColumn"> getTimestampColumn

```java
Optional<TimestampColumn> getTimestampColumn()
```

Used when:

* `SchemaKSourceFactory` is requested to [buildWindowedStream](SchemaKSourceFactory.md#buildWindowedStream), [buildStream](SchemaKSourceFactory.md#buildStream), [buildWindowedTable](SchemaKSourceFactory.md#buildWindowedTable), [buildTable](SchemaKSourceFactory.md#buildTable)
* `KsqlStream` is requested to `with`
* `KsqlTable` is requested to `with`
* `StructuredDataSource` is requested for the [PROPERTIES](StructuredDataSource.md#PROPERTIES)
* `SourceDescriptionFactory` is requested to `create`

### <span id="isSource"> isSource

```java
boolean isSource()
```

Used when:

* `AlterSourceFactory` is requested to `create`
* `CreateSourceFactory` is requested to [throwIfCreateOrReplaceOnSourceStreamOrTable](CreateSourceFactory.md#throwIfCreateOrReplaceOnSourceStreamOrTable)
* `DropSourceFactory` is requested to `create`
* `EngineExecutor` is requested to [execute a plan](EngineExecutor.md#execute)
* `TopicDeleteInjector` is requested to `inject`
* `KsqlStream` is requested to `with`
* `KsqlTable` is requested to `with`
* `InsertValuesExecutor` is requested to `getDataSource`

### <span id="with"> with

```java
DataSource with(
  String sql,
  LogicalSchema schema)
```

Creates a copy of this `DataSource` with the given SQL appended and the given `LogicalSchema`

Used when:

* `DdlCommandExec.Executor` is requested to [execute AlterSourceCommand](DdlCommandExec.Executor.md#executeAlterSource)

## Implementations

* [StructuredDataSource](StructuredDataSource.md)

## <span id="DataSourceType"> DataSourceType

### <span id="KSTREAM"><span id="STREAM"> KSTREAM

### <span id="KTABLE"><span id="TABLE"> KTABLE
