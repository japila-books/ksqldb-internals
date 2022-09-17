# SchemaKSourceFactory

## <span id="buildSource"> Building Source SchemaKStream

```java
SchemaKStream<?> buildSource(
  PlanBuildContext buildContext,
  DataSource dataSource,
  QueryContext.Stacker contextStacker)
```

`buildSource` requests the given [DataSource](DataSource.md) whether it is windowed or not and the type.

For `KSTREAM` type, `buildSource` builds a [windowed](#buildWindowedStream) or [regular](#buildStream) stream based on whether it is windowed or not, respectively.

For `KTABLE` type, `buildSource` builds a [windowed](#buildWindowedTable) or [regular](#buildTable) table based on whether it is windowed or not, respectively.

---

`buildSource` is used when:

* `DataSourceNode` is requested for a [SchemaKStream](planner/DataSourceNode.md#buildStream)

### <span id="buildStream"> Creating SchemaKStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext,
  DataSource dataSource,
  Stacker contextStacker)
```

`buildStream` [creates a new SchemaKStream](#schemaKStream) with a [StreamSource](ExecutionStepFactory.md#streamSource).

### <span id="buildWindowedStream"> Creating Windowed SchemaKStream

```java
SchemaKStream<?> buildWindowedStream(
  PlanBuildContext buildContext,
  DataSource dataSource,
  Stacker contextStacker)
```

`buildWindowedStream`...FIXME

## <span id="schemaKStream"> Creating SchemaKStream

```java
SchemaKStream<K> schemaKStream(
  PlanBuildContext buildContext,
  LogicalSchema schema,
  KeyFormat keyFormat,
  SourceStep<KStreamHolder<K>> streamSource)
```

`schemaKStream` creates a [SchemaKStream](SchemaKStream.md).

---

`schemaKStream` is used when:

* `SchemaKSourceFactory` is requested for a [windowed](#buildWindowedStream) and [non-windowed SchemaKStream](#buildStream)
