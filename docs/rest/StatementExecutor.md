# StatementExecutor

`StatementExecutor` is an [abstraction](#contract) of [executors](#implementations) of [Statement](../Statement.md)s.

`StatementExecutor` is a `FunctionalInterface` ([Java]({{ java.api }}/java/lang/FunctionalInterface.html)).

## Contract

### <span id="execute"> Executing Statement

```java
StatementExecutorResponse execute(
  ConfiguredStatement<T> statement,
  SessionProperties sessionProperties,
  KsqlExecutionContext executionContext,
  ServiceContext serviceContext)
```

Used when:

* `CustomExecutors` is requested to [execute](CustomExecutors.md#execute)
* `RequestHandler` is requested to [execute a SQL statement](RequestHandler.md#executeStatement)

## Implementations

* _many_
