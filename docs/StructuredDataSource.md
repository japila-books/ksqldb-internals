# StructuredDataSource

`StructuredDataSource` is an extension of the [DataSource](DataSource.md) abstraction for [data sources](#implementations) with a [schema](#schema).

## Implementations

* [KsqlStream](KsqlStream.md)
* `KsqlTable`

## Creating Instance

`StructuredDataSource` takes the following to be created:

* <span id="sqlExpression"> SQL expression
* <span id="dataSourceName"> Data source name
* <span id="schema"> `LogicalSchema`
* <span id="tsExtractionPolicy"> `TimestampColumn`
* <span id="dataSourceType"> [DataSourceType](DataSource.md#DataSourceType)
* <span id="casTarget"> `casTarget` flag
* <span id="ksqlTopic"> `KsqlTopic`
* <span id="isSource"> `isSource` flag

!!! note "Abstract Class"
    `StructuredDataSource` is an abstract class and cannot be created directly. It is created indirectly for the [concrete StructuredDataSources](#implementations).
