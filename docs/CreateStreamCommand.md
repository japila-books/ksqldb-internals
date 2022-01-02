# CreateStreamCommand

`CreateStreamCommand` is a [CreateSourceCommand](CreateSourceCommand.md).

## Creating Instance

`CreateStreamCommand` takes the following to be created:

* <span id="sourceName"> Source Name
* <span id="schema"> `LogicalSchema`
* <span id="timestampColumn"> `TimestampColumn`
* <span id="topicName"> Topic Name
* <span id="formats"> `Formats`
* <span id="windowInfo"> `WindowInfo`
* <span id="orReplace"> `orReplace` flag
* <span id="isSource"> `isSource` flag

`CreateStreamCommand` is created when:

* `CreateSourceFactory` is requested to [create a CreateStreamCommand](CreateSourceFactory.md#createStreamCommand)

## <span id="execute"> Executing Command

```java
DdlCommandResult execute(
  Executor executor)
```

`execute` requests the given [Executor](Executor.md) to [execute this CreateStreamCommand](Executor.md#executeCreateStream).

`execute` is part of the [DdlCommand](DdlCommand.md#execute) abstraction.
