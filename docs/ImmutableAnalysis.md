# ImmutableAnalysis

`ImmutableAnalysis` is an [abstraction](#contract) of [query analysers](#implementations).

## Contract (Subset)

### <span id="getJoin"> getJoin

```java
List<JoinInfo> getJoin()
```

See [Analysis](Analysis.md#getJoin)

Used when:

* `LogicalPlanner` is requested to [build a source node](LogicalPlanner.md#buildSourceNode)

### <span id="getSelectItems"> getSelectItems

```java
List<SelectItem> getSelectItems()
```

See [Analysis](Analysis.md#getSelectItems)

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
