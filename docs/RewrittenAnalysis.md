# RewrittenAnalysis

`RewrittenAnalysis` is an [ImmutableAnalysis](ImmutableAnalysis.md).

## Creating Instance

`RewrittenAnalysis` takes the following to be created:

* <span id="original"> Original [ImmutableAnalysis](ImmutableAnalysis.md)
* [Rewriter Function](#rewriter)

`RewrittenAnalysis` is created when:

* `KsqlEngine` is requested to [analyzeQueryWithNoOutputTopic](KsqlEngine.md#analyzeQueryWithNoOutputTopic)
* `LogicalPlanner` is [created](planner/LogicalPlanner.md#analysis)

### <span id="rewriter"> Rewriter Function

```java
BiFunction<Expression, Context<Void>, Optional<Expression>> rewriter
```

`RewrittenAnalysis` is given a rewriter function when [created](#creating-instance).

The rewriter function is used in [rewrite](#rewrite).

## <span id="getSelectItems"> getSelectItems

```java
List<SelectItem> getSelectItems()
```

`getSelectItems` is part of the [ImmutableAnalysis](ImmutableAnalysis.md#getSelectItems) abstraction.

---

`getSelectItems`...FIXME

## <span id="rewrite"> rewrite

```java
ColumnName rewrite(
  ColumnName name)
<T extends Expression> T rewrite(
  T expression)
```

`rewrite`...FIXME

---

`rewrite` is used when:

* `RewrittenAnalysis` is requested to [getSelectColumnNames](#getSelectColumnNames), [getSelectItems](#getSelectItems), [getDefaultArgument](#getDefaultArgument), [rewriteOptional](#rewriteOptional), [rewriteList](#rewriteList)
