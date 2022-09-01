# AlterSystemProperty

`AlterSystemProperty` is a [Statement](Statement.md) for [ALTER SYSTEM](AstBuilder_Visitor.md#visitAlterSystemProperty) statements.

## Creating Instance

`AlterSystemProperty` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="propertyName"> Property Name
* <span id="propertyValue"> Property Value

`AlterSystemProperty` is created when:

* `AstBuilder.Visitor` is requested to [visitAlterSystemProperty](AstBuilder_Visitor.md#visitAlterSystemProperty)
