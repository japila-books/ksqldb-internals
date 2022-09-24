# SystemColumns

## <span id="pseudoColumns"> Pseudo Columns

Column Name | Type | Version | isDisallowedForInsertValues | isDisallowedInPullAndScalablePushQueries
------------|------|---------|-----------------------------------------------------------------------
 `ROWTIME` | `BIGINT` | 0 | `false` | `false`
 `ROWPARTITION` | `INTEGER` | 1 | `true` | `true`
 `ROWOFFSET` | `BIGINT` | 1 | `true` | `true`

Used when:

* [isPseudoColumn](#isPseudoColumn)
* [pseudoColumnNames](#pseudoColumnNames)
* [mustBeMaterializedForTableJoins](#mustBeMaterializedForTableJoins)
* [isDisallowedForInsertValues](#isDisallowedForInsertValues)
* [isDisallowedInPullOrScalablePushQueries](#isDisallowedInPullOrScalablePushQueries)

## <span id="isPseudoColumn"> isPseudoColumn

```java
boolean isPseudoColumn(
  ColumnName columnName,
  int pseudoColumnVersion)
boolean isPseudoColumn(
  ColumnName columnName,
  KsqlConfig ksqlConfig)
boolean isPseudoColumn(
  ColumnName columnName,
  boolean rowpartitionRowoffsetEnabled)
```

`isPseudoColumn` [validates](#validatePseudoColumnVersion) the given `pseudoColumnVersion` and checks if the `columnName` is among the [pseudoColumns](#pseudoColumns) (by version and name).

* `DataSourceNode` is requested to [nonKnownColumn](../planner/DataSourceNode.md#nonKnownColumn)
* `PlanNode` is requested to [orderColumns](../planner/PlanNode.md#orderColumns)
* `CodeGenRunner.Visitor` is requested to `fieldNotFoundErrorMessage`
* `QueryValidatorUtil` is requested to `validateNoUserColumnsWithSameNameAsPseudoColumns`
* `SourceSchemas` is requested to `matchesNonValueField`
* `DataSourceExtractor` is requested to [isClashingColumnName](../DataSourceExtractor.md#isClashingColumnName) and [hasColumn](../DataSourceExtractor.md#hasColumn)
