# CreateTable

`CreateTable` is a [CreateSource](CreateSource.md) and a [ExecutableDdlStatement](ExecutableDdlStatement.md) that represents [CREATE TABLE](AstBuilder_Visitor.md#visitCreateTable) and [ASSERT TABLE](AstBuilder_Visitor.md#visitAssertTable) statements.

## Creating Instance

`CreateTable` takes the following to be created:

* <span id="location"> Node Location
* <span id="name"> Source Name
* <span id="elements"> `TableElements`
* <span id="orReplace"> `orReplace` flag
* <span id="notExists"> `notExists` flag
* <span id="properties"> [CreateSourceProperties](CreateSourceProperties.md)
* <span id="isSource"> `isSource` flag

`CreateTable` is created when:

* `AstBuilder.Visitor` is requested to parse [CREATE TABLE](AstBuilder_Visitor.md#visitCreateTable) and [ASSERT TABLE](AstBuilder_Visitor.md#visitAssertTable) statements
