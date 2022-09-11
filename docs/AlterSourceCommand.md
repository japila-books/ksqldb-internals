# AlterSourceCommand

`AlterSourceCommand` is a [DdlCommand](DdlCommand.md) that represents [AlterSource](parser/AlterSource.md) statement at execution.

`AlterSourceCommand` is executed using [DdlCommandExec.Executor](DdlCommandExec.Executor.md#executeAlterSource).

## Creating Instance

`AlterSourceCommand` takes the following to be created:

* <span id="sourceName"> Source name
* <span id="ksqlType"> KSQL type
* <span id="newColumns"> New columns

`AlterSourceCommand` is created when:

* `AlterSourceFactory` is requested to [create an AlterSourceCommand](AlterSourceFactory.md#create) (for [AlterSource](parser/AlterSource.md) statement)
