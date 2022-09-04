# StreamSelectBuilder

## <span id="build"> Building KStreamHolder

```java
KStreamHolder<K> build(
  KStreamHolder<K> stream,
  StreamSelect<K> step,
  RuntimeBuildContext buildContext)
```

`build`...FIXME

---

`build` is used when:

* `KSPlanBuilder` is requested to [visit a StreamSelect](KSPlanBuilder.md#visitStreamSelect)
