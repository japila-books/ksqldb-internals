# CreateStream

`CreateStream` is a [CreateSource](CreateSource.md) and a [ExecutableDdlStatement](ExecutableDdlStatement.md) that represents [CREATE STREAM](AstBuilder.md#visitCreateStream) and [ASSERT STREAM](AstBuilder.md#visitAssertStream) statements.

## Creating Instance

`CreateStream` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="name"> `SourceName`
* <span id="elements"> `TableElements`
* <span id="orReplace"> `orReplace` flag
* <span id="notExists"> `notExists` flag
* <span id="properties"> [CreateSourceProperties](CreateSourceProperties.md)
* <span id="isSource"> `isSource` flag

`CreateStream` is created when:

* `AstBuilder.Visitor` is requested to parse [CREATE STREAM](AstBuilder.md#visitCreateStream) and [ASSERT STREAM](AstBuilder.md#visitAssertStream) statements
