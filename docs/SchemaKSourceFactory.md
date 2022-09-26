# SchemaKSourceFactory

`SchemaKSourceFactory` is used as a factory of [source SchemaKStreams](#buildSource) (for a [DataSource](DataSource.md)) for [DataSourceNode](planner/DataSourceNode.md#schemaKStreamFactory).

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

### <span id="buildStream"> SchemaKStream

```java
SchemaKStream<?> buildStream(
  PlanBuildContext buildContext,
  DataSource dataSource,
  Stacker contextStacker)
```

`buildStream` [creates a new SchemaKStream](#schemaKStream) for a new [StreamSource](ExecutionStepFactory.md#streamSource) (with [pseudoColumnVersionToUse](#determinePseudoColumnVersionToUse)).

### <span id="buildWindowedStream"> Windowed SchemaKStream

```java
SchemaKStream<?> buildWindowedStream(
  PlanBuildContext buildContext,
  DataSource dataSource,
  Stacker contextStacker)
```

`buildWindowedStream`...FIXME

### <span id="buildTable"> SchemaKTable

```java
SchemaKTable<?> buildTable(
  PlanBuildContext buildContext,
  DataSource dataSource,
  Stacker contextStacker)
```

`buildTable` requests the given [DataSource](DataSource.md) for the [KeyFormat](KeyFormat.md) (through the [KsqlTopic](DataSource.md#getKsqlTopic)).

`buildTable` [determinePseudoColumnVersionToUse](#determinePseudoColumnVersionToUse).

`buildTable` creates a [TableSource](ExecutionStepFactory.md#tableSource) (or legacy [TableSourceV1](ExecutionStepFactory.md#tableSourceV1)) if [ksql.rowpartition.rowoffset.enabled](KsqlConfig.md#KSQL_ROWPARTITION_ROWOFFSET_ENABLED) is enabled or not, respectively.

In the end, `buildTable` [resolves the schema](#resolveSchema) and [creates a SchemaKTable](#schemaKTable) (for the `SourceStep<KTableHolder<GenericKey>>`).

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

## <span id="determinePseudoColumnVersionToUse"> determinePseudoColumnVersionToUse

```java
int determinePseudoColumnVersionToUse(
  PlanBuildContext buildContext)
```

`determinePseudoColumnVersionToUse`...FIXME

---

`determinePseudoColumnVersionToUse` is used when:

* `SchemaKSourceFactory` is requested to [build a source SchemaKStream](#buildSource) ([buildWindowedStream](#buildWindowedStream), [buildStream](#buildStream), [buildWindowedTable](#buildWindowedTable), [buildTable](#buildTable))

## <span id="schemaKTable"> Creating SchemaKTable

```java
<K> SchemaKTable<K> schemaKTable(
  PlanBuildContext buildContext,
  LogicalSchema schema,
  KeyFormat keyFormat,
  SourceStep<KTableHolder<K>> tableSource)
```

`schemaKTable` creates a [SchemaKTable](SchemaKTable.md).

---

`schemaKTable` is used when:

* `SchemaKSourceFactory` is requested to build a [windowed](#buildWindowedTable) or [regular table](#buildTable)

## <span id="resolveSchema"> Resolving LogicalSchema (of ExecutionStep)

```java
LogicalSchema resolveSchema(
  PlanBuildContext buildContext,
  ExecutionStep<?> step,
  DataSource dataSource)
```

`resolveSchema` creates a [StepSchemaResolver](StepSchemaResolver.md) (for the given `PlanBuildContext`) to [resolve](StepSchemaResolver.md#resolve) the [LogicalSchema](DataSource.md#getSchema) (from the given [DataSource](DataSource.md)) of the given [ExecutionStep](ExecutionStep.md).
