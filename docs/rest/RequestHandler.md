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

The `KsqlEngine` is used for [executing SQL Statements](#execute) (to [execute a Statement](#executeStatement) and [isVariableSubstitutionEnabled](#isVariableSubstitutionEnabled)).

## <span id="distributor"> DistributingExecutor

`RequestHandler` is given a [DistributingExecutor](DistributingExecutor.md) when [created](#creating-instance).

The `DistributingExecutor` is used for [executing SQL statements](#executeStatement).

## <span id="execute"> Executing SQL Statements (execute)

```java
KsqlEntityList execute(
  KsqlSecurityContext securityContext,
  List<ParsedStatement> statements,
  SessionProperties sessionProperties)
```

For every SQL statement (in the given `statements`), `execute` requests the [KsqlEngine](#ksqlEngine) to [prepare it for execution](../KsqlEngine.md#prepare) (possibly with [variable substitution](#isVariableSubstitutionEnabled)) and then [executes it](#executeStatement).

`execute` is used when:

* `KsqlResource` is requested to [handle a REST request to execute SQL statements](KsqlResource.md#handleKsqlStatements) and [terminate the cluster](KsqlResource.md#terminateCluster)

### <span id="executeStatement"> Executing Statement

```java
<T extends Statement> Optional<KsqlEntity> executeStatement(
  KsqlSecurityContext securityContext,
  PreparedStatement<T> prepared,
  SessionProperties sessionProperties,
  KsqlEntityList entities)
```

`executeStatement` requests the given `PreparedStatement` for the [Statement](../parser/Statement.md) and its Java class that is used to request the [CommandQueueSync](#commandQueueSync) to `waitFor`.

`executeStatement` creates a `ConfiguredStatement` for the given `PreparedStatement` (with a new `SessionConfig`).

`executeStatement` looks up the [StatementExecutor](StatementExecutor.md) for the `Statement` class in the [CustomExecutors](#customExecutors) registry (or defaults to the [DistributingExecutor](#distributor) to [execute the statement](DistributingExecutor.md#execute)).

`executeStatement` requests the `StatementExecutor` to [execute the statement](StatementExecutor.md#execute).

Unless handled, `executeStatement` requests the [DistributingExecutor](#distributor) to [execute the statement](DistributingExecutor.md#execute).

### <span id="isVariableSubstitutionEnabled"> isVariableSubstitutionEnabled

```java
boolean isVariableSubstitutionEnabled(
  SessionProperties sessionProperties)
```

`isVariableSubstitutionEnabled` is positive (`true`) when [ksql.variable.substitution.enable](../KsqlConfig.md#KSQL_VARIABLE_SUBSTITUTION_ENABLE) is `true` in the given `SessionProperties` or the [KsqlConfig](../KsqlEngine.md#getKsqlConfig) (of the [KsqlEngine](#ksqlEngine)).
