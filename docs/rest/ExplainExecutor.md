# ExplainExecutor

`ExplainExecutor` is a utility to [explain statements](#explain).

## <span id="execute"> execute

```java
StatementExecutorResponse execute(
  ConfiguredStatement<Explain> statement,
  SessionProperties sessionProperties,
  KsqlExecutionContext executionContext,
  ServiceContext serviceContext)
```

`execute` creates a `StatementExecutorResponse` with [explain](#explain) (for the given arguments).

`execute` is used when:

* `CustomExecutors` is requested to [handle an Explain query](CustomExecutors.md#Explain)
