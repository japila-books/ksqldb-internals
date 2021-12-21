# Analysis

`Analysis` is an [ImmutableAnalysis](ImmutableAnalysis.md).

## <span id="joinInfo"> joinInfo

`Analysis` defines `joinInfo` registry of `JoinInfo`s that are added in [addJoin](#addJoin).

### <span id="addJoin"> addJoin

```java
void addJoin(
  JoinInfo joinInfo)
```

`addJoin` adds the given `JoinInfo` to the [joinInfo](#joinInfo) registry.

`addJoin` is used when:

* `Analyzer.Visitor` is requested to [visitJoinedSource](Analyzer.md#visitJoinedSource)

### <span id="isJoin"> isJoin

```java
boolean isJoin()
```

`isJoin` is `true` when there is a `JoinInfo` in the [joinInfo](#joinInfo) registry.

`isJoin` is part of the [ImmutableAnalysis](ImmutableAnalysis.md#isJoin) abstraction.

## <span id="addDataSource"> addDataSource

```java
void addDataSource(
  SourceName alias,
  DataSource dataSource)
```

`addDataSource`...FIXME

`addDataSource` is used when:

* `Analyzer.Visitor` is requested to [visitAliasedRelation](Analyzer.md#visitAliasedRelation)
