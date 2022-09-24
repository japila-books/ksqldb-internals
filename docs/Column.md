# Column

## Namespaces

### <span id="KEY"> KEY

### <span id="VALUE"> VALUE

### <span id="HEADERS"> HEADERS

Indicates a [header column](features/header-columns.md)

Headers columns are printed out in [toString](#toString) when [headerKey](#headerKey) is available.

Used when:

* `LogicalSchema` is requested to [headers](LogicalSchema.md#headers), [isHeaderColumn](LogicalSchema.md#isHeaderColumn), [rebuildWithPseudoAndKeyColsInValue](LogicalSchema.md#rebuildWithPseudoAndKeyColsInValue), [rebuildWithoutPseudoAndKeyColsInValue](LogicalSchema.md#rebuildWithoutPseudoAndKeyColsInValue), [nonPseudoHeaderAndKeyColsAsValueCols](LogicalSchema.md#nonPseudoHeaderAndKeyColsAsValueCols)
* `LogicalSchema.Builder` is [created](LogicalSchema.Builder.md), and requested to [headerColumn](LogicalSchema.Builder.md#headerColumn), [addColumn](LogicalSchema.Builder.md#addColumn)
* `EntityUtil` is requested to `fieldType`
