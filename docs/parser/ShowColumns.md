# ShowColumns

`ShowColumns` is a [StatementWithExtendedClause](StatementWithExtendedClause.md) that represents the following KSQL statement:

```antlr
DESCRIBE sourceName EXTENDED?
```

`ShowColumns` is executed using [ListSourceExecutor](../rest/ListSourceExecutor.md#columns).

## Creating Instance

`ShowColumns` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="table"> Source Name
* [isExtended](#isExtended) flag

`ShowColumns` is created when:

* `AstBuilder.Visitor` is requested to [parse DESCRIBE SOURCE statement](AstBuilder.Visitor.md#visitShowColumns)

### <span id="isExtended"> isExtended Flag

`ShowColumns` is given `isExtended` flag when [created](#creating-instance).

The `isExtended` flag is used when:

* `ListSourceExecutor` is requested to [columns](../rest/ListSourceExecutor.md#columns)

## <span id="accept"> accept

```java
<R, C> R accept(
  AstVisitor<R, C> visitor,
  C context)
```

`accept` is part of the [Statement](Statement.md#accept) abstraction.

---

`accept` requests the [AstVisitor](AstVisitor.md) to [visitShowColumns](#visitShowColumns) (with this `ShowColumns` instance).
