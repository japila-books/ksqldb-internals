# SchemaKSourceFactory

## <span id="buildSource"> Building SchemaKStream

```java
SchemaKStream<?> buildSource(
  PlanBuildContext buildContext,
  DataSource dataSource,
  QueryContext.Stacker contextStacker)
```

`buildSource`...FIXME

`buildSource` is used when:

* `DataSourceNode` is [created](DataSourceNode.md#schemaKStreamFactory)

### <span id="buildStream"> buildStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext,
  DataSource dataSource,
  Stacker contextStacker)
```

`buildStream`...FIXME

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
