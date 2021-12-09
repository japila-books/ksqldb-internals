# ExecutionStep

`ExecutionStep<S>` is an [abstraction](#contract) of [execution steps](#implementations) to [build S](#build).

## Contract (Subset)

### <span id="build"> Building S

```java
S build(
  PlanBuilder planBuilder,
  PlanInfo planInfo)
S build(
  PlanBuilder planBuilder)
```

Used when:

* `QueryBuilder` is requested to [buildQueryImplementation](QueryBuilder.md#buildQueryImplementation)

## Implementations

* [SourceStep](SourceStep.md)
* _many others_
