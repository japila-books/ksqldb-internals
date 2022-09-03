# CreateSourceCommand

`CreateSourceCommand` is an extension of the [DdlCommand](DdlCommand.md) abstraction for [DDL commands](#implementations) to create sources ([streams](CreateStreamCommand.md) and [tables](CreateTableCommand.md)).

## Implementations

* [CreateStreamCommand](CreateStreamCommand.md)
* [CreateTableCommand](CreateTableCommand.md)

## Creating Instance

`CreateSourceCommand` takes the following to be created:

* <span id="sourceName"> `SourceName`
* <span id="schema"> `LogicalSchema`
* <span id="timestampColumn"> `TimestampColumn`
* <span id="topicName"> Topic Name
* <span id="formats"> `Formats`
* <span id="windowInfo"> `WindowInfo`
* <span id="orReplace"> `orReplace` flag
* <span id="isSource"> `isSource` flag

!!! note "Abstract Class"
    `CreateSourceCommand` is an abstract class and cannot be created directly. It is created indirectly for the [concrete CreateSourceCommands](#implementations).
