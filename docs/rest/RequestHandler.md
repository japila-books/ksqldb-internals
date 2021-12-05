# RequestHandler

## Creating Instance

`RequestHandler` takes the following to be created:

* <span id="customExecutors"> [CustomExecutors](CustomExecutors.md#EXECUTOR_MAP)
* <span id="distributor"> `DistributingExecutor`
* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* <span id="commandQueueSync"> `CommandQueueSync`

`RequestHandler` is created when:

* `KsqlResource` is requested to [configure](KsqlResource.md#configure)

## <span id="distributor"> DistributingExecutor

`RequestHandler` is given a `DistributingExecutor` when [created](#creating-instance).

The `DistributingExecutor` is used when [executeStatement](#executeStatement).

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

### <span id="executeStatement"> executeStatement

```java
<T extends Statement> Optional<KsqlEntity> executeStatement(
  KsqlSecurityContext securityContext,
  PreparedStatement<T> prepared,
  SessionProperties sessionProperties,
  KsqlEntityList entities)
```

`executeStatement`...FIXME
