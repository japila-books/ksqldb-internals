# PlanBuilder

`PlanBuilder` is an [abstraction](#contract) of [query plan builders](#implementations) for building a query from an execution plan.

## Contract (Subset)

### <span id="visitStreamSource"> Visiting StreamSource

```java
KStreamHolder<GenericKey> visitStreamSource(
  StreamSource streamSource,
  PlanInfo planInfo)
```

Visits a [StreamSource](StreamSource.md)

See [KSPlanBuilder](KSPlanBuilder.md#visitStreamSource)

Used when:

* `StreamSource` is requested to [build a KStreamHolder](StreamSource.md#build)

## Implementations

* [KSPlanBuilder](KSPlanBuilder.md)
