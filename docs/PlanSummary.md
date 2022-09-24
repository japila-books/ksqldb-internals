# PlanSummary

`PlanSummary` is a utility to [build a plan summary](#summarize) of a query (represented by an [ExecutionStep](ExecutionStep.md)).

## <span id="OP_NAME"> Operator Names

```java
Map<Class<? extends ExecutionStep>, String> OP_NAME
```

`PlanSummary` defines `OP_NAME` registry of [ExecutionStep](ExecutionStep.md)s and their names to use for a [summary](#summarize) of execution plans.

ExecutionStep | Operator Name
--------------|---------
 `StreamAggregate` | `AGGREGATE`
 `StreamWindowedAggregate` | `AGGREGATE`
 `StreamFilter` | `FILTER`
 `StreamFlatMap` | `FLAT_MAP`
 `StreamGroupByV1` | `GROUP_BY`
 `StreamGroupBy` | `GROUP_BY`
 `StreamGroupByKey` | `GROUP_BY`
 `StreamSelect` | `PROJECT`
 `StreamSelectKeyV1` | `REKEY`
 `StreamSelectKey` | `REKEY`
 `StreamSink` | `SINK`
 `StreamSource` | `SOURCE`
 `StreamStreamJoin` | `JOIN`
 `StreamTableJoin` | `JOIN`
 `WindowedStreamSource` | `SOURCE`
 `TableAggregate` | `AGGREGATE`
 `TableFilter` | `FILTER`
 `TableGroupByV1` | `GROUP_BY`
 `TableGroupBy` | `GROUP_BY`
 `TableSelect` | `PROJECT`
 `TableSelectKey` | `REKEY`
 `TableSink` | `SINK`
 `TableTableJoin` | `JOIN`
 `ForeignKeyTableTableJoin` | `JOIN`
 `TableSourceV1` | `SOURCE`
 `TableSource` | `SOURCE`
 `TableSuppress` | `SUPPRESS`
 `WindowedTableSource` | `SOURCE`

## <span id="summarize"> summarize

```java
String summarize(
  ExecutionStep<?> step) // (1)!
StepSummary summarize(
  ExecutionStep<?> step,
  String indent)
```

1. Uses an empty indent

`summarize` builds plan summaries of the [sources](ExecutionStep.md#getSources) (of the given [ExecutionStep](ExecutionStep.md)).

`summarize` [builds the LogicalSchema](#getSchema) of the execution plan (with the given [ExecutionStep](ExecutionStep.md) and the plan summaries of the sources).

`summarize` [builds the query logger name](QueryLoggerUtil.md#queryLoggerName).

In the end, `summarize` combines it all (alongside the sources).

---

`summarize` is used when:

* `EngineExecutor` is requested to [build a query plan summary](EngineExecutor.md#buildPlanSummary)
