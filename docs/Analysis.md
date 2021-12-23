# Analysis

`Analysis` is an [ImmutableAnalysis](ImmutableAnalysis.md).

## Creating Instance

`Analysis` takes the following to be created:

* <span id="refinementInfo"> `RefinementInfo`
* <span id="rowpartitionRowoffsetEnabled"> `rowpartitionRowoffsetEnabled` flag
* <span id="pullLimitClauseEnabled"> `pullLimitClauseEnabled` flag

`Analysis` is created when:

* `Analyzer.Visitor` is [created](Analyzer.md#analysis)

## <span id="selectItems"> SelectItems

`Analysis` defines `joinInfo` registry of `JoinInfo`s that are added in [addJoin](#addJoin).

### <span id="addSelectItem"> addSelectItem

```java
void addSelectItem(
  SelectItem selectItem)
```

`addSelectItem` adds the given `SelectItem` to the [selectItems](#selectItems) registry.

`addSelectItem` is used when:

* `Analyzer.Visitor` is requested to [visitSelect](Analyzer.md#visitSelect)

### <span id="getSelectItems"> getSelectItems

```java
List<SelectItem> getSelectItems()
```

`getSelectItems` returns the [selectItems](#selectItems).

`getSelectItems` is part of the [ImmutableAnalysis](ImmutableAnalysis.md#getSelectItems) abstraction.

## <span id="joinInfo"> JoinInfos

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
