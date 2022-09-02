# CreateStream

`CreateStream` is a [CreateSource](CreateSource.md) and a [ExecutableDdlStatement](ExecutableDdlStatement.md) that represents [CREATE STREAM](AstBuilder.md#visitCreateStream) and [ASSERT STREAM](AstBuilder.md#visitAssertStream) statements.

`CreateStream` is handled by:

* [CommandFactories](../CommandFactories.md) is used to [handleCreateStream](../CommandFactories.md#handleCreateStream)
* `CreateSourceFactory` is used to [createStreamCommand](../CreateSourceFactory.md#createStreamCommand)
* `CommandIdAssigner` is requested to `getTopicStreamCommandId`

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
* `CreateStream` is requested to [copyWith](#copyWith)

## <span id="copyWith"> copyWith

```java
CreateSource copyWith(
  TableElements elements,
  CreateSourceProperties properties
)
```

`copyWith` is part of the [CreateSource](CreateSource.md#copyWith) abstraction.

---

`copyWith` creates a new [CreateStream](#creating-instance) with the given `TableElements` and `CreateSourceProperties`.

## Planning

`EngineExecutor` is requested to [plan CREATE STREAM statements](../EngineExecutor.md#plan)

## Execution

* [DistributingExecutor](../rest/DistributingExecutor.md#execute)
* [StatementExecutor](../rest/StatementExecutor.md#handleExecutableDdl)
