# ExecutionStep

`ExecutionStep<S>` is an [abstraction](#contract) of [execution steps](#implementations) (_physical plans_) to [build S](#build).

`S` can be one of the following:

* `KStreamHolder`
* `KTableHolder`
* `KGroupedStreamHolder`
* `KGroupedTableHolder`

## Contract

### <span id="build"> Building S

```java
S build(
  PlanBuilder planBuilder) // (1)!
S build(
  PlanBuilder planBuilder,
  PlanInfo planInfo)
```

1. Uses a [PlanInfoExtractor](PlanInfoExtractor.md) to [extract a PlanInfo](#extractPlanInfo)

Used when:

* `QueryBuilder` is requested for a [query implementation](QueryBuilder.md#buildQueryImplementation)

### <span id="extractPlanInfo"> Extracting PlanInfo

```java
PlanInfo extractPlanInfo(
  PlanInfoExtractor planInfoExtractor)
```

Used when:

* `EngineExecutor` is requested to [planQuery](EngineExecutor.md#planQuery)
* `ExecutionStep` is requested to [build](#build)
* `PlanInfoExtractor` is requested to [visitRepartitionStep](PlanInfoExtractor.md#visitRepartitionStep), [visitJoinStep](PlanInfoExtractor.md#visitJoinStep) and [visitSingleSourceStep](PlanInfoExtractor.md#visitSingleSourceStep)

## Implementations

* [SourceStep](SourceStep.md)
* [StreamSelect](StreamSelect.md)
* _others_
