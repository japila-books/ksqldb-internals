# Persistent Queries

**Persistent Queries** (per `PersistentQueryType`) can be the following queries:

* [CREATE_SOURCE](#CREATE_SOURCE)
* [CREATE STREAM AS SELECT](parser/AstBuilder.Visitor.md#visitCreateStreamAs)
* [CREATE TABLE AS SELECT](parser/AstBuilder.Visitor.md#visitCreateTableAs)
* [INSERT INTO](parser/AstBuilder.Visitor.md#visitInsertInto)

When [Analyzer](analyzer/Analyzer.md) is requested to [analyze a query](analyzer/Analyzer.md#analyze) it creates a [Visitor](analyzer/Analyzer.md#Visitor) with a flag to indicate whether the sink is defined or not for persistent queries.

## <span id="CREATE_SOURCE"> CREATE_SOURCE

`CREATE_SOURCE`s don't write to a topic (so `EngineExecutor` does not have to [check for read-only topics](EngineExecutor.md#execute) the other query types could attempt to write to).

It is forbidden to terminate `CREATE_SOURCE` queries when linked to a source table (and `ValidatedCommandFactory` throws a [KsqlStatementException](rest/ValidatedCommandFactory.md)):

```text
Cannot terminate query '[queryId]' because it is linked to a source table.
```

`EngineExecutor` will not execute `CREATE_SOURCE` plans when [ksql.source.table.materialization.enabled](KsqlConfig.md#KSQL_SOURCE_TABLE_MATERIALIZATION_ENABLED) is disabled and prints out the following INFO message to the logs instead:

```text
Source table query '[statementText]' won't be materialized because 'ksql.source.table.materialization.enabled' is disabled.
```

`CREATE_SOURCE` is used when:

* `KsqlPlanV1` is requested for the [getPersistentQueryType](KsqlPlanV1.md#getPersistentQueryType) (with the [queryPlan](KsqlPlanV1.md#queryPlan) and the [ddlCommand](KsqlPlanV1.md#ddlCommand) specified as a source `CreateTableCommand`)
* `QueryBuilder` is requested to [buildPersistentQueryInDedicatedRuntime](QueryBuilder.md#buildPersistentQueryInDedicatedRuntime) and [buildPersistentQueryInSharedRuntime](QueryBuilder.md#buildPersistentQueryInSharedRuntime)
* `QueryRegistryImpl` is requested to [registerPersistentQuery](QueryRegistryImpl.md#registerPersistentQuery) and [unregisterQuery](QueryRegistryImpl.md#unregisterQuery)