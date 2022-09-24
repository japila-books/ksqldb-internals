# LogicalSchema.Builder

`LogicalSchema.Builder` is used to [create a schema](#build).

## Creating Instance

`LogicalSchema.Builder` takes the following to be created:

* [Columns](#columns)

`LogicalSchema.Builder` is created when:

* `LogicalSchema` is requested to [builder](LogicalSchema.md#builder) and [asBuilder](LogicalSchema.md#asBuilder)

## <span id="columns"> Columns

`LogicalSchema.Builder` is given [Column](Column.md)s that are added to `columns` registry (using [keyColumn](#keyColumn), [headerColumn](#headerColumn) and [valueColumn](#valueColumn) for `KEY`, `HEADERS` and `VALUE` columns, respectively).

The columns are then used to [build a LogicalSchema](#build).

## <span id="build"> Creating LogicalSchema

```java
LogicalSchema build()
```

`build` creates a [LogicalSchema](LogicalSchema.md) with the [columns](#columns).

---

`build` is used when:

* FIXME

## <span id="headerColumn"> headerColumn

```java
Builder headerColumn(
  ColumnName columnName,
  Optional<String> headerKey)
Builder headerColumn(
  Column column)
```

`headerColumn`...FIXME

---

`headerColumn` is used when:

* `LogicalSchema.Builder` is [created](#creating-instance) and [headerColumns](#headerColumns)
* `TableElements` is requested to [toLogicalSchema](parser/TableElements.md#toLogicalSchema)

## <span id="headerColumns"> headerColumns

```java
Builder headerColumns(
  Iterable<? extends Column> columns)
```

`headerColumns` [adds every column](#headerColumn) in the given `columns` and returns this `LogicalSchema.Builder`.

---

`headerColumns` is used when:

* `QueryProjectNode` is requested to [buildPullQuerySelectStarSchema](planner/QueryProjectNode.md#buildPullQuerySelectStarSchema)

## <span id="addColumn"> addColumn

```java
void addColumn(
  Column column)
```

`addColumn`...FIXME

---

`addColumn` is used when:

* `LogicalSchema.Builder` is requested to add a [header](#headerColumn), [key](#keyColumn), [value](#valueColumn) column
