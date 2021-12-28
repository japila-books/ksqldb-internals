# ExplainExecutor

`ExplainExecutor` is a utility to [explain queries or statements](#explain).

## <span id="execute"> Executing Explain

```java
StatementExecutorResponse execute(
  ConfiguredStatement<Explain> statement,
  SessionProperties sessionProperties,
  KsqlExecutionContext executionContext,
  ServiceContext serviceContext)
```

`execute` creates a `StatementExecutorResponse` with [explain](#explain) (for the given arguments).

`execute` is used when:

* `CustomExecutors` is requested to [handle an Explain statement](CustomExecutors.md#Explain)

### <span id="explain"> Explaining Query or Statement

```java
QueryDescriptionEntity explain(
  ServiceContext serviceContext,
  ConfiguredStatement<Explain> statement,
  KsqlExecutionContext executionContext,
  SessionProperties sessionProperties)
```

`explain` looks up a query ID from the given `ConfiguredStatement`.

If found, `explain` [explainQuery](#explainQuery). Otherwise, `explain` [explainStatement](#explainStatement).

### <span id="explainStatement"> Explaining Statement

```java
QueryDescription explainStatement(
  ConfiguredStatement<Explain> explain,
  KsqlExecutionContext executionContext,
  ServiceContext serviceContext)
```

`explainStatement`...FIXME

### <span id="explainQuery"> Explaining Query

```java
QueryDescription explainQuery(
  String queryId,
  KsqlExecutionContext executionContext,
  SessionProperties sessionProperties)
```

`explainQuery`...FIXME
