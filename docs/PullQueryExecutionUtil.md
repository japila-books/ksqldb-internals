# PullQueryExecutionUtil

## <span id="findMaterializingQuery"> findMaterializingQuery

```java
PersistentQueryMetadata findMaterializingQuery(
  EngineContext engineContext,
  ImmutableAnalysis analysis)
```

`findMaterializingQuery`...FIXME

---

`findMaterializingQuery` is used when:

* `EngineExecutor` is requested to [build a physical plan for a pull query](EngineExecutor.md#buildPullPhysicalPlan)

### <span id="notMaterializedException"> notMaterializedException

```java
KsqlException notMaterializedException(
  SourceName sourceTable)
```

`notMaterializedException` throws a `KsqlException`:

```text
The [sourceTable] table isn't queryable. To derive a queryable table,
you can do 'CREATE TABLE QUERYABLE_[tableName] AS SELECT * FROM [tableName]'.
See https://cnfl.io/queries for more info.
Add EMIT CHANGES if you intended to issue a push query.
```
