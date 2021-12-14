# ImmutableAnalysis

`ImmutableAnalysis` is an [abstraction](#contract) of [query analysers](#implementations).

## Contract (Subset)

### <span id="isJoin"> isJoin

```java
boolean isJoin()
```

See [Analysis](Analysis.md#isJoin)

Used when:

* `PullQueryValidator` is [created](PullQueryValidator.md#RULES)
* `LogicalPlanner` is requested to [build a source node](LogicalPlanner.md#buildSourceNode)

## Implementations

* [Analysis](Analysis.md)