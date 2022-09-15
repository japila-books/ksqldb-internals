# CreateStreamAsSelect

`CreateStreamAsSelect` is a [CreateAsSelect](CreateAsSelect.md) that represents [CREATE STREAM AS SELECT](AstBuilder_Visitor.md#visitCreateStreamAs) KSQL command.

## Creating Instance

`CreateStreamAsSelect` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="name"> Source Name
* <span id="query"> [Query](Query.md)
* <span id="notExists"> `notExists` flag
* <span id="orReplace"> `orReplace` flag
* <span id="properties"> [CreateSourceAsProperties](CreateSourceAsProperties.md)

`CreateStreamAsSelect` is created when:

* `StatementRewriter.Rewriter` is requested to `visitCreateStreamAsSelect`
* `AstBuilder.Visitor` is requested to [parse CREATE STREAM AS SELECT statement](AstBuilder_Visitor.md#visitCreateStreamAs)
