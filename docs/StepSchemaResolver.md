# StepSchemaResolver

## <span id="HANDLERS"> Handlers

`StepSchemaResolver` defines `HANDLERS` registry of [ExecutionStep](ExecutionStep.md)s and their handlers.

ExecutionStep | Handler
--------------|----------
 [StreamSelectKeyV1](StreamSelectKeyV1.md) | [handleStreamSelectKeyV1](StepSchemaResolver.md#handleStreamSelectKeyV1)
 _others_ |

## <span id="resolve"> Resolving LogicalSchema

```java
LogicalSchema resolve(
  ExecutionStep<?> step,
  LogicalSchema schema)
```

`resolve` executes the handler of the given [ExecutionStep](ExecutionStep.md) (in the [HANDLERS](#HANDLERS)) registry.

---

`resolve` is used when:

* `SchemaKGroupedStream` is requested to `resolveSchema`
* `SchemaKSourceFactory` is requested to [resolve a schema (of an ExecutionStep)](SchemaKSourceFactory.md#resolveSchema)
* `SchemaKStream` is requested to [resolve a schema (of an ExecutionStep)](SchemaKStream.md#resolveSchema)
* `PlanSummary` is requested to [get a schema](PlanSummary.md#getSchema)
* `StreamSelectKeyBuilderV1` is requested to [build a KStreamHolder for a StreamSelectKeyV1](StreamSelectKeyBuilderV1.md#build)
