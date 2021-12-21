# Analyzer

## Creating Instance

`Analyzer` takes the following to be created:

* <span id="metaStore"> [MetaStore](MetaStore.md)
* <span id="topicPrefix"> Topic Prefix
* <span id="rowpartitionRowoffsetEnabled"> `rowpartitionRowoffsetEnabled` flag
* <span id="pullLimitClauseEnabled"> [ksql.query.pull.limit.clause.enabled](KsqlConfig.md#KSQL_QUERY_PULL_LIMIT_CLAUSE_ENABLED) configuration property

`Analyzer` is created when:

* `QueryAnalyzer` is [created](QueryAnalyzer.md#analyzer)

![Analyzer](images/Analyzer.png)
