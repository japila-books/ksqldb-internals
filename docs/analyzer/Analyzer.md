# Analyzer

`Analyzer` is used by [QueryAnalyzer](QueryAnalyzer.md#analyzer) to [analyze KSQL query statements](#analyze) (and produce an [Analysis](Analysis.md)).

<figure markdown>
  ![Analyzer](../images/Analyzer.png)
</figure>

## Creating Instance

`Analyzer` takes the following to be created:

* <span id="metaStore"> [MetaStore](../MetaStore.md)
* <span id="topicPrefix"> Topic Prefix
* <span id="rowpartitionRowoffsetEnabled"> `rowpartitionRowoffsetEnabled` flag
* <span id="pullLimitClauseEnabled"> [ksql.query.pull.limit.clause.enabled](../KsqlConfig.md#KSQL_QUERY_PULL_LIMIT_CLAUSE_ENABLED)

`Analyzer` is created along with [QueryAnalyzer](QueryAnalyzer.md#analyzer).

## <span id="analyze"> Analysing KSQL Query Statement

```java
Analysis analyze(
  Query query,
  Optional<Sink> sink)
```

`analyze` creates an [Analyzer.Visitor](Analyzer.Visitor.md) (with the given [Query](../parser/Query.md) and whether the [Sink](../parser/Sink.md) is present for the [persistent](Analyzer.Visitor.md#persistent) flag).

`analyze` requests the `Analyzer.Visitor` for the following:

1. [Process](../parser/AstVisitor.md#process) the given `Query`
1. [analyzeNonStdOutSink](Analyzer.Visitor.md#analyzeNonStdOutSink) only if the [Sink](../parser/Sink.md) is defined
1. [Validate](Analyzer.Visitor.md#validate)

In the end, `analyze` requests the `Analyzer.Visitor` for the [Analysis](Analyzer.Visitor.md#analysis).

---

`analyze` is used when:

* `QueryAnalyzer` is requested to [analyze a Query statement](QueryAnalyzer.md#analyze)
