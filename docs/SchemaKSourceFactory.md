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

### <span id="buildStream"> Building SchemaKStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext,
  DataSource dataSource,
  Stacker contextStacker)
```

`buildStream`...FIXME
