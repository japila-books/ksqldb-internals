# QueryAnalyzer

## Creating Instance

`QueryAnalyzer` takes the following to be created:

* <span id="metaStore"> [MetaStore](MetaStore.md)
* <span id="outputTopicPrefix"> Output Topic Prefix
* <span id="rowpartitionRowoffsetEnabled"> `rowpartitionRowoffsetEnabled` flag
* <span id="pullLimitClauseEnabled"> `pullLimitClauseEnabled` flag

`QueryAnalyzer` is created when:

* `KsqlEngine` is requested to [analyzeQueryWithNoOutputTopic](KsqlEngine.md#analyzeQueryWithNoOutputTopic)
* `QueryEngine` utility is used to [buildQueryLogicalPlan](QueryEngine.md#buildQueryLogicalPlan)

## <span id="analyzer"> Analyzer

Unless given, `QueryAnalyzer` creates an [Analyzer](Analyzer.md) when [created](#creating-instance).

The `Analyzer` is used to [analyze a query](#analyze).

## <span id="analyze"> Query Analysis

```java
Analysis analyze(
  Query query,
  Optional<Sink> sink)
```

![QueryAnalyzer.analyze](images/QueryAnalyzer-analyze.png)

`analyze` requests the [Analyzer](#analyzer) to [analyze the query](Analyzer.md#analyze).

`analyze` requests the [pull](#pullQueryValidator) or [push query validator](#pushQueryValidator) to [validate the analysis](QueryValidator.md#validate) based on whether it is a [pull query](parser/Query.md#isPullQuery) or not, respectively.

---

The optional `Sink` can only be defined when `QueryEngine` is requested to [buildQueryLogicalPlan](QueryEngine.md#buildQueryLogicalPlan).

---

`analyze` is used when:

* `KsqlEngine` is requested to [analyzeQueryWithNoOutputTopic](KsqlEngine.md#analyzeQueryWithNoOutputTopic)
* `QueryEngine` is requested to [buildQueryLogicalPlan](QueryEngine.md#buildQueryLogicalPlan)
