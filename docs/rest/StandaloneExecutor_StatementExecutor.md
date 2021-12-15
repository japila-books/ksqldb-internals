# StatementExecutor

`StatementExecutor` is a `private static final class` of [StandaloneExecutor](StandaloneExecutor.md) with [statement handlers](#HANDLERS).

## Creating Instance

`StatementExecutor` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](../ServiceContext.md)
* <span id="executionContext"> [KsqlExecutionContext](../KsqlExecutionContext.md)
* <span id="injector"> `Injector`
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)

`StatementExecutor` is created when:

* `StandaloneExecutor` is requested to [processesQueryFile](StandaloneExecutor.md#processesQueryFile) and [validateStatements](#validateStatements)

## <span id="HANDLERS"> Statement Handlers

`StatementExecutor` creates `HANDLERS` collection of handlers (_functions_) of [Statement](../Statement.md)s.

SQL | Statement Class | Handler
----|-----------------|---------
 <span id="SetProperty"> SET | `SetProperty` | `StatementExecutor::handleSetProperty`
 UNSET | `UnsetProperty` | `StatementExecutor::handleUnsetProperty`
 CREATE STREAM | [CreateStream](../CreateStream.md) | `StatementExecutor::handleExecutableDdl`
 CREATE TABLE | `CreateTable` | `StatementExecutor::handleExecutableDdl`
 REGISTER TYPE | `RegisterType` | `StatementExecutor::handleExecutableDdl`
 <span id="CreateStreamAsSelect"> CREATE STREAM AS SELECT | `CreateStreamAsSelect` | [handlePersistentQuery](#handlePersistentQuery)
 <span id="CreateTableAsSelect"> CREATE TABLE AS SELECT | `CreateTableAsSelect` | [handlePersistentQuery](#handlePersistentQuery)
 <span id="InsertInto"> INSERT INTO | [InsertInto](../InsertInto.md) | [handlePersistentQuery](#handlePersistentQuery)

The `HANDLERS` is used in [execute](#execute) and [generateSupportedMessage](#generateSupportedMessage).

## <span id="handlePersistentQuery"> handlePersistentQuery

```java
void handlePersistentQuery(
  ConfiguredStatement<?> statement)
```

`handlePersistentQuery` requests the [KsqlExecutionContext](#executionContext) to [execute the given statement](../KsqlExecutionContext.md#execute) (in the [ServiceContext](#serviceContext)) to produce a [QueryMetadata](../QueryMetadata.md).

`handlePersistentQuery` makes sure that the `QueryMetadata` is a `PersistentQueryMetadata` or throws a `KsqlStatementException`:

```text
Could not build the query
```

`handlePersistentQuery` is used when:

* `StatementExecutor` is requested to handle [CREATE STREAM AS SELECT](#CreateStreamAsSelect), [CREATE TABLE AS SELECT](#CreateTableAsSelect) and [INSERT INTO](#InsertInto) SQL statements
