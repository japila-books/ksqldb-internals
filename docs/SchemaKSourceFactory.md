# SchemaKSourceFactory

## <span id="buildSource"> Building SchemaKStream

```java
SchemaKStream<?> buildSource(
  PlanBuildContext buildContext,
  DataSource dataSource,
  QueryContext.Stacker contextStacker)
```

`buildSource` requests the given [DataSource](DataSource.md) whether it is windowed or not and the type.

For `KSTREAM` type, `buildSource` builds a [windowed](#buildWindowedStream) or [regular](#buildStream) stream based on whether it is windowed or not, respectively.

For `KTABLE` type, `buildSource` builds a [windowed](#buildWindowedTable) or [regular](#buildTable) table based on whether it is windowed or not, respectively.

`buildSource` is used when:

* `DataSourceNode` is [created](planner/DataSourceNode.md#schemaKStreamFactory)

### <span id="buildStream"> buildStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext,
  DataSource dataSource,
  Stacker contextStacker)
```

`buildStream` [creates a new SchemaKStream](#schemaKStream) with a [StreamSource](ExecutionStepFactory.md#streamSource).

### <span id="buildWindowedStream"> buildWindowedStream

```java
SchemaKStream<?> buildWindowedStream(
  PlanBuildContext buildContext,
  DataSource dataSource,
  Stacker contextStacker)
```

`buildWindowedStream`...FIXME

### <span id="schemaKStream"> schemaKStream

```java
SchemaKStream<K> schemaKStream(
  PlanBuildContext buildContext,
  LogicalSchema schema,
  KeyFormat keyFormat,
  SourceStep<KStreamHolder<K>> streamSource)
```

`schemaKStream` creates a [SchemaKStream](SchemaKStream.md).
