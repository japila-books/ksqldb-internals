# Analysis

`Analysis` is an [ImmutableAnalysis](ImmutableAnalysis.md).

## Creating Instance

`Analysis` takes the following to be created:

* <span id="refinementInfo"> `RefinementInfo`
* <span id="rowpartitionRowoffsetEnabled"> `rowpartitionRowoffsetEnabled` flag
* <span id="pullLimitClauseEnabled"> `pullLimitClauseEnabled` flag

`Analysis` is created when:

* `Analyzer.Visitor` is [created](Analyzer.Visitor.md#analysis)

## <span id="tableFunctions"> TableFunctions

`Analysis` defines `tableFunctions` registry of `FunctionCall`s that are added in [addTableFunction](#addTableFunction).

### <span id="addTableFunction"> addTableFunction

```java
void addTableFunction(
  FunctionCall functionCall)
```

`addTableFunction` adds the given `FunctionCall` to the [tableFunctions](#tableFunctions) registry.

`addTableFunction` is used when:

* `Analyzer.Visitor` is requested to [visitTableFunctions](Analyzer.md#visitTableFunctions)

### <span id="getTableFunctions"> getTableFunctions

```java
List<FunctionCall> getTableFunctions()
```

`getTableFunctions` returns the [tableFunctions](#tableFunctions).

`getTableFunctions` is part of the [ImmutableAnalysis](ImmutableAnalysis.md#getTableFunctions) abstraction.

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

`getSelectItems` is part of the [ImmutableAnalysis](ImmutableAnalysis.md#getSelectItems) abstraction.

---

`getSelectItems` returns the [selectItems](#selectItems).

## <span id="into"><span id="setInto"> Into

```java
Optional<Into> into
```

`Analysis` defines `into` registry for an `Into` that is empty by default.

!!! note "Into"
    `Into` holds the name of a source and the associated topic (to be created or existing).

`into` is set when `Analyzer.Visitor` is requested to [analyzeNonStdOutSink](Analyzer.Visitor.md#analyzeNonStdOutSink).

### <span id="getInto"> getInto

```java
Optional<Into> getInto()
```

`getInto` is part of the [ImmutableAnalysis](ImmutableAnalysis.md#getInto) abstraction.

---

`getInto` returns the [Into](#into).

### <span id="setInto"> setInto

```java
void setInto(
  Into into)
```

`setInto` sets the [Into](#into).

## <span id="joinInfo"> JoinInfos

`Analysis` defines `joinInfo` registry of `JoinInfo`s that are added in [addJoin](#addJoin).

### <span id="addJoin"> addJoin

```java
void addJoin(
  JoinInfo joinInfo)
```

`addJoin` adds the given `JoinInfo` to the [joinInfo](#joinInfo) registry.

---

`addJoin` is used when:

* `Analyzer.Visitor` is requested to [visitJoinedSource](Analyzer.md#visitJoinedSource)

### <span id="isJoin"> isJoin

```java
boolean isJoin()
```

`isJoin` is part of the [ImmutableAnalysis](ImmutableAnalysis.md#isJoin) abstraction.

---

`isJoin` is `true` when there is a `JoinInfo` in the [joinInfo](#joinInfo) registry.

## <span id="addDataSource"> addDataSource

```java
void addDataSource(
  SourceName alias,
  DataSource dataSource)
```

`addDataSource`...FIXME

`addDataSource` is used when:

* `Analyzer.Visitor` is requested to [visitAliasedRelation](Analyzer.md#visitAliasedRelation)
