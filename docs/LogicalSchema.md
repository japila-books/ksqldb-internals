# LogicalSchema

`LogicalSchema` represents a logical schema of a [StructuredDataSource](StructuredDataSource.md#getSchema).

`LogicalSchema` is made up of [columns](#columns).

## Creating Instance

`LogicalSchema` takes the following to be created:

* [Columns](#columns)

`LogicalSchema` is created when:

* `LogicalSchema` is requested to [withKeyColsOnly](#withKeyColsOnly), [rebuildWithPseudoAndKeyColsInValue](#rebuildWithPseudoAndKeyColsInValue), [rebuildWithoutPseudoAndKeyColsInValue](#rebuildWithoutPseudoAndKeyColsInValue), [rebuildWithPseudoColumnsToMaterialize](#rebuildWithPseudoColumnsToMaterialize)
* `LogicalSchema.Builder` is requested to [build a LogicalSchema](LogicalSchema.Builder.md#build)

### <span id="columns"> Columns

`LogicalSchema` is given [Column](Column.md)s when [created](#creating-instance).

Used when:

* [asBuilder](#asBuilder)
* [byNamespace](#byNamespace)
* [findColumnMatching](#findColumnMatching)

## <span id="key"> Key Schema

```java
List<Column> key()
```

`key` [groups columns by namespace](#byNamespace) and takes out the [Column](Column.md)s in [KEY](Column.md#KEY) namespace.

## <span id="value"> Value Schema

```java
List<Column> value()
```

`value` [groups columns by namespace](#byNamespace) and takes out the [Column](Column.md)s in [VALUE](Column.md#VALUE) namespace.

## <span id="headers"> Header Columns

```java
List<Column> headers()
```

`headers` [groups columns by namespace](#byNamespace) and takes out the [Column](Column.md)s in [HEADERS](Column.md#HEADERS) namespace.

`headers` is used when:

* `LogicalSchema` is requested to [rebuildWithPseudoColumnsToMaterialize](#rebuildWithPseudoColumnsToMaterialize)
* `AstSanitizer.RewriterPlugin` is requested to [validate INSERT INTO statement](AstSanitizer.RewriterPlugin.md#visitInsertInto) (to assert that the target datasource has no header columns)
* `QueryProjectNode` is requested to [buildPullQuerySelectStarSchema](planner/QueryProjectNode.md#buildPullQuerySelectStarSchema)
* `DistributingExecutor` is requested to [validate INSERT INTO statement](rest/DistributingExecutor.md#validateInsertIntoQueries) (to assert that the target datasource has no header columns)
* `InsertValuesExecutor` is requested to validate INSERT VALUES statement (to assert that the target datasource has no header columns)
* `SourceBuilder` is requested to [build a KTable](SourceBuilder.md#buildKTable)
* `SourceBuilderV1` is requested to [build a KTable](SourceBuilderV1.md#buildKTable) and a [KStream](SourceBuilderV1.md#buildKStream)

## <span id="byNamespace"> byNamespace

```java
Map<Namespace, List<Column>> byNamespace()
```

`byNamespace`...FIXME

---

`byNamespace` is used when:

* `LogicalSchema` is requested to [key](#key), [value](#value), [headers](#headers), [withKeyColsOnly](#withKeyColsOnly), [rebuildWithPseudoAndKeyColsInValue](#rebuildWithPseudoAndKeyColsInValue), [rebuildWithoutPseudoAndKeyColsInValue](#rebuildWithoutPseudoAndKeyColsInValue), [rebuildWithPseudoColumnsToMaterialize](#rebuildWithPseudoColumnsToMaterialize)

## <span id="findColumnMatching"> findColumnMatching

```java
Optional<Column> findColumnMatching(
  Predicate<Column> predicate)
```

`findColumnMatching`...FIXME

---

`findColumnMatching` is used when:

* `LogicalSchema` is requested to [findColumn](#findColumn), [findValueColumn](#findValueColumn), [isKeyColumn](#isKeyColumn), [isHeaderColumn](#isHeaderColumn), [nonPseudoHeaderAndKeyColsAsValueCols](#nonPseudoHeaderAndKeyColsAsValueCols)
