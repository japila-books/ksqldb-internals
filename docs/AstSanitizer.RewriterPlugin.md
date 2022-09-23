# AstSanitizer.RewriterPlugin

`AstSanitizer.RewriterPlugin` is a [AstVisitor](parser/AstVisitor.md) to [validate INSERT INTO statements](#visitInsertInto) (and [column aliases](#visitSingleColumn)).

!!! note "private static final class"
    `AstSanitizer.RewriterPlugin` is a `private static final class` of [AstSanitizer](AstSanitizer.md).

## Creating Instance

`AstSanitizer.RewriterPlugin` takes the following to be created:

* <span id="metaStore"> [MetaStore](MetaStore.md)
* <span id="dataSourceExtractor"> [DataSourceExtractor](DataSourceExtractor.md)

`AstSanitizer.RewriterPlugin` is created when:

* `AstSanitizer` is requested to [sanitize a KSQL statement](AstSanitizer.md#sanitize)

## <span id="visitInsertInto"> Validating INSERT INTO Statement

```java
Optional<AstNode> visitInsertInto(
  InsertInto node,
  StatementRewriter.Context<SanitizerContext> ctx)
```

`visitInsertInto` is part of the [AstVisitor](parser/AstVisitor.md#visitInsertInto) abstraction.

---

`visitInsertInto` throws a `UnknownSourceException` when the [target DataSource](parser/InsertInto.md#getTarget) (of the given [InsertInto](parser/InsertInto.md)) could not be found in the [MetaStore](#metaStore):

```text
Source [target] does not exist.
```

`visitInsertInto` throws a `KsqlException` when the [getDataSourceType](DataSource.md#getDataSourceType) of the [target DataSource](parser/InsertInto.md#getTarget) (of the given [InsertInto](parser/InsertInto.md)) is not [KSTREAM](DataSource.md#KSTREAM):

```text
INSERT INTO can only be used to insert into a stream.
[target] is a table.
```

`visitInsertInto` throws a `KsqlException` when the [target DataSource](parser/InsertInto.md#getTarget) (of the given [InsertInto](parser/InsertInto.md)) has got header columns:

```text
Cannot insert into [target] because it has header columns
```
