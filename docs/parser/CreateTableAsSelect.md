# CreateTableAsSelect

`CreateTableAsSelect` is a [CreateAsSelect](CreateAsSelect.md) that represents [CREATE TABLE AS SELECT](AstBuilder_Visitor.md#visitCreateTableAs) KSQL command.

In [headless](../headless/index.md) execution mode, `CreateTableAsSelect` is executed using [StatementExecutor](../headless/StandaloneExecutor_StatementExecutor.md#handlePersistentQuery).

## Creating Instance

`CreateTableAsSelect` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="name"> Source Name
* <span id="query"> [Query](Query.md)
* <span id="notExists"> `notExists` flag
* <span id="orReplace"> `orReplace` flag
* <span id="properties"> [CreateSourceAsProperties](CreateSourceAsProperties.md)

`CreateTableAsSelect` is created when:

* `StatementRewriter.Rewriter` is requested to `visitCreateTableAsSelect`
* `AstBuilder.Visitor` is requested to [parse CREATE TABLE AS SELECT statement](AstBuilder_Visitor.md#visitCreateTableAs)
