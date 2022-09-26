# StreamSelectKeyBuilderV1

## <span id="build"> build

```java
KStreamHolder<GenericKey> build(
  KStreamHolder<?> stream,
  StreamSelectKeyV1 selectKey,
  RuntimeBuildContext buildContext)
```

`build` requests the `KStreamHolder` for the [LogicalSchema](LogicalSchema.md).

`build` [builds an ExpressionEvaluator](#buildExpressionEvaluator).

`build`...FIXME

---

`build` is used when:

* `KSPlanBuilder` is requested to [visit a StreamSelectKey](KSPlanBuilder.md#visitStreamSelectKey)
