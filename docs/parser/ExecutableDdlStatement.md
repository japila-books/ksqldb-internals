# ExecutableDdlStatement

`ExecutableDdlStatement` is an extension of the [DdlStatement](DdlStatement.md) abstraction and a marker interface for [executable DDL statements](#implementations).

`ExecutableDdlStatement` is among the **executable statements** for [KsqlEngine](../KsqlEngine.md#isExecutableStatement).

`ExecutableDdlStatement`s are planned for execution by [EngineExecutor](../EngineExecutor.md#plan).

## Implementations

* `AlterSource`
* [CreateStream](CreateStream.md)
* [CreateTable](CreateTable.md)
* `DropStream`
* `DropTable`
* `DropType`
* `RegisterType`
