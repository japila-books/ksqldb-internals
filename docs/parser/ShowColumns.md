# ShowColumns

`ShowColumns` is a [StatementWithExtendedClause](StatementWithExtendedClause.md).

## Creating Instance

`ShowColumns` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="table"> Source Name
* <span id="isExtended"> `isExtended` flag

`ShowColumns` is created when:

* `AstBuilder.Visitor` is requested to [parse DESCRIBE SOURCE statement](AstBuilder.Visitor.md#visitShowColumns)
