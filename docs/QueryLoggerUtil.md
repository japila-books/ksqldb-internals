# QueryLoggerUtil

## <span id="queryLoggerName"> queryLoggerName

```java
String queryLoggerName(
  QueryId queryId,
  QueryContext queryContext)
```

`queryLoggerName` builds a query logger name for the given `QueryId` and `QueryContext` (separated by `.`).

---

`queryLoggerName` is used when:

* `QueryBuilder` is requested to [getUncaughtExceptionProcessingLogger](QueryBuilder.md#getUncaughtExceptionProcessingLogger)
* `PlanSummary` is requested to [summarize](PlanSummary.md#summarize)
* `RuntimeBuildContext` is requested for [ProcessingLogger](RuntimeBuildContext.md#getProcessingLogger), and to[buildKeySerde](RuntimeBuildContext.md#buildKeySerde), [buildKeySerde](RuntimeBuildContext.md#buildKeySerde), [buildValueSerde](RuntimeBuildContext.md#buildValueSerde)
