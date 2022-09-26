# SystemColumns

## <span id="ROWKEY_NAME"><span id="ROWKEY"> ROWKEY

## <span id="handleStreamSelectKeyV1"> handleStreamSelectKeyV1

```java
LogicalSchema handleStreamSelectKeyV1(
  LogicalSchema sourceSchema,
  StreamSelectKeyV1 step)
```

`handleStreamSelectKeyV1` requests the given [StreamSelectKeyV1](../StreamSelectKeyV1.md) for the [key expression](../StreamSelectKeyV1.md#getKeyExpression).

`handleStreamSelectKeyV1` creates a `ExpressionTypeManager` to [getExpressionSqlType](#getExpressionSqlType) for the key expression (that gives a [SqlType](../types/SqlType.md)).

In the end, `handleStreamSelectKeyV1` creates a [LogicalSchema](../LogicalSchema.md) with the following columns:

* [ROWKEY](#ROWKEY) of the `SqlType`
* [Value columns](../LogicalSchema.md#value) of the given source [LogicalSchema](../LogicalSchema.md)

---

`handleStreamSelectKeyV1` is used when:

* `StepSchemaResolver` is requested for the [HANDLERS](../StepSchemaResolver.md#HANDLERS) (of [StreamSelectKeyV1](../StreamSelectKeyV1.md))

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
