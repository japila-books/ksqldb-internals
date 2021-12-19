# ExecutionStep

`ExecutionStep<S>` is an [abstraction](#contract) of [execution steps](#implementations) (_physical plans_) to [build S](#build).

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

* `ForeignKeyTableTableJoin`
* [SourceStep](SourceStep.md)
* `StreamAggregate`
* `StreamFilter`
* `StreamFlatMap`
* `StreamGroupBy`
* `StreamGroupByKey`
* `StreamGroupByV1`
* `StreamSelect`
* `StreamSelectKey`
* `StreamSelectKeyV1`
* `StreamSink`
* `StreamStreamJoin`
* `StreamTableJoin`
* `StreamWindowedAggregate`
* `TableAggregate`
* `TableFilter`
* `TableGroupBy`
* `TableGroupByV1`
* `TableSelect`
* `TableSelectKey`
* `TableSink`
* `TableSuppress`
* `TableTableJoin`
