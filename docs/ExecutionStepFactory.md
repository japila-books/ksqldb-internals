# ExecutionStepFactory

## <span id="streamSource"> Creating StreamSource

```java
StreamSource streamSource(
  QueryContext.Stacker stacker,
  LogicalSchema sourceSchema,
  String topicName,
  Formats formats,
  Optional<TimestampColumn> timestampColumn,
  int pseudoColumnVersion)
```

`streamSource` creates a [StreamSource](StreamSource.md) (with `ExecutionStepPropertiesV1`).

---

`streamSource` is used when:

* `SchemaKSourceFactory` is requested to [buildStream](SchemaKSourceFactory.md#buildStream)
