# InsertInto

`InsertInto` is a [Statement](Statement.md) and a [QueryContainer](QueryContainer.md).

## Creating Instance

`InsertInto` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="target"> Source Name
* <span id="query"> [Query](Query.md)
* <span id="properties"> `InsertIntoProperties`

`InsertInto` is created when:

* `AstBuilder.Visitor` is requested to [parse INSERT INTO statement](AstBuilder_Visitor.md#visitInsertInto)
* `StatementRewriter.Rewriter` is requested to `visitInsertInto`
