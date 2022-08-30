# ValidatedCommandFactory

`ValidatedCommandFactory` is a collection of utilities to [create validated commands](#create) (that are safe to enqueue onto the command queue).

## <span id="create"> Creating Validated Command

```java
Command create(
  ConfiguredStatement<? extends Statement> statement,
  KsqlExecutionContext context)
Command create(
  ConfiguredStatement<? extends Statement> statement,
  ServiceContext serviceContext,
  KsqlExecutionContext context
```

`create` creates a [Command](Command.md).

`create`...FIXME

---

`create` is used when:

* `DistributingExecutor` is requested to [execute a statement](DistributingExecutor.md#execute)
* `RequestValidator` is requested to [validate](RequestValidator.md#validate)

### <span id="createCommand"> Creating Command

```java
Command create(
  ConfiguredStatement<? extends Statement> statement,
  KsqlExecutionContext context)
Command create(
  ConfiguredStatement<? extends Statement> statement,
  ServiceContext serviceContext,
  KsqlExecutionContext context
```

`createCommand`...FIXME

### <span id="createForAlterSystemQuery"> createForAlterSystemQuery

```java
Command createForAlterSystemQuery(
  ConfiguredStatement<? extends Statement> statement,
  KsqlExecutionContext context)
```

`createForAlterSystemQuery`...FIXME

### <span id="createForPlannedQuery"> createForPlannedQuery

```java
Command createForPlannedQuery(
  ConfiguredStatement<? extends Statement> statement,
  ServiceContext serviceContext,
  KsqlExecutionContext context)
```

`createForPlannedQuery` requests the given [KsqlExecutionContext](../KsqlExecutionContext.md) to [plan a DDL/DML statement](../KsqlExecutionContext.md#plan) (that gives a [KsqlPlan](../KsqlPlan.md)).

`createForPlannedQuery` [creates a ConfiguredKsqlPlan](../ConfiguredKsqlPlan.md#of).

`createForPlannedQuery`...FIXME

### <span id="createForTerminateQuery"> createForTerminateQuery

```java
Command createForTerminateQuery(
  ConfiguredStatement<? extends Statement> statement,
  KsqlExecutionContext context)
```

`createForTerminateQuery` assumes that the given `statement` is a `TerminateQuery`.

`createForTerminateQuery` handles the following cases:

1. No `queryId` defined to close all the running persistent queries
1. `queryId`s with the `transient_` query name prefix
1. Non-`CREATE_SOURCE` persistent queries

---

With no `queryId` defined, `createForTerminateQuery` requests [all the running persistent queries](../KsqlExecutionContext.md#getPersistentQueries) (in the given [KsqlExecutionContext](../KsqlExecutionContext.md)) to [close](../QueryMetadata.md#close) and returns a [Command](Command.md#of) with the given `statement`.

For a `queryId` that contains the `transient_` query name prefix, `createForTerminateQuery` returns a [Command](Command.md#of) with the given `statement`.

For all the other `queryId`s, `createForTerminateQuery` [looks up the query](../KsqlExecutionContext.md#getPersistentQuery) (in the given [KsqlExecutionContext](../KsqlExecutionContext.md)) to [close it](../QueryMetadata.md#close).

In the end, `createForTerminateQuery` returns a [Command](Command.md#of) with the given `statement`.

---

`createForTerminateQuery` throws a `KsqlStatementException` for `CREATE_SOURCE` queries:

```text
Cannot terminate query '[queryId]' because it is linked to a source table.
```
