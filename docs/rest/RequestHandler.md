# RequestHandler

## Creating Instance

`RequestHandler` takes the following to be created:

* <span id="customExecutors"> [CustomExecutors](CustomExecutors.md#EXECUTOR_MAP)
* [DistributingExecutor](#distributor)
* [KsqlEngine](#ksqlEngine)
* <span id="commandQueueSync"> `CommandQueueSync`

`RequestHandler` is created when:

* `KsqlResource` is requested to [configure](KsqlResource.md#configure)

## <span id="ksqlEngine"> KsqlEngine

`RequestHandler` is given a [KsqlEngine](../KsqlEngine.md) when [created](#creating-instance).

The `KsqlEngine` is used for the following:

* [Executing SQL Statements](#execute) (and [executeStatement](#executeStatement))
* [isVariableSubstitutionEnabled](#isVariableSubstitutionEnabled)

## <span id="distributor"> DistributingExecutor

`RequestHandler` is given a [DistributingExecutor](DistributingExecutor.md) when [created](#creating-instance).

The `DistributingExecutor` is used for [executing SQL statements](#executeStatement).

## <span id="execute"> Executing SQL Statements

```java
KsqlEntityList execute(
  KsqlSecurityContext securityContext,
  List<ParsedStatement> statements,
  SessionProperties sessionProperties)
```

`execute`...FIXME

`execute` [executes the SQL statement](#executeStatement).

`execute`...FIXME

`execute` is used when:

* `KsqlResource` is requested to [terminateCluster](KsqlResource.md#terminateCluster) and [handleKsqlStatements](KsqlResource.md#handleKsqlStatements)

### <span id="executeStatement"> Executing Statement

```java
<T extends Statement> Optional<KsqlEntity> executeStatement(
  KsqlSecurityContext securityContext,
  PreparedStatement<T> prepared,
  SessionProperties sessionProperties,
  KsqlEntityList entities)
```

`executeStatement` requests the given `PreparedStatement` for the [Statement](../Statement.md) and its Java class that is used to request the [CommandQueueSync](#commandQueueSync) to `waitFor`.

`executeStatement` creates a `ConfiguredStatement` for the given `PreparedStatement` (with a new `SessionConfig`).

`executeStatement` looks up the [StatementExecutor](StatementExecutor.md) for the `Statement` class in the [CustomExecutors](#customExecutors) registry (or defaults to the [DistributingExecutor](#distributor) to [execute the statement](DistributingExecutor.md#execute)).

`executeStatement` requests the `StatementExecutor` to [execute the statement](StatementExecutor.md#execute).

Unless handled, `executeStatement` requests the [DistributingExecutor](#distributor) to [execute the statement](DistributingExecutor.md#execute).
