# ListStreams

`ListStreams` is a [StatementWithExtendedClause](StatementWithExtendedClause.md) that represents the following KSQL statement:

```antlr
(LIST | SHOW) STREAMS EXTENDED?
```

`ListStreams` is executed using [ListSourceExecutor](../rest/ListSourceExecutor.md#streams).

## Creating Instance

`ListStreams` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="showExtended"> `showExtended` flag

`ListStreams` is created when:

* `AstBuilder.Visitor` is requested to [parse LIST STREAMS statement](AstBuilder.Visitor.md#visitListStreams)
