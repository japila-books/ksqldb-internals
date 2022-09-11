# AlterSource

`AlterSource` is a [Statement](Statement.md) and a [ExecutableDdlStatement](ExecutableDdlStatement.md) that represents [ALTER SOURCE](AstBuilder_Visitor.md#visitAlterSource) KSQL command.

```text
ALTER (STREAM | TABLE) sourceName alterOption (',' alterOption)*
```

`AlterSource` is converted into [AlterSourceCommand](../AlterSourceCommand.md) (using [CommandFactories](../CommandFactories.md#handleAlterSource) and [AlterSourceFactory](../AlterSourceFactory.md)).

## Creating Instance

`AlterSource` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="name"> `SourceName`
* <span id="dataSourceType"> [DataSourceType](../DataSource.md#DataSourceType)
* <span id="alterOptions"> `AlterOption`s

`AlterSource` is created when:

* `AstBuilder.Visitor` is requested to [parse ALTER SOURCE statement](AstBuilder_Visitor.md#visitAlterSource)
