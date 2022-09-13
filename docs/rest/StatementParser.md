# StatementParser

`StatementParser` is used to [parse KSQL statements](#parseSingleStatement) in the following:

* [InteractiveStatementExecutor](InteractiveStatementExecutor.md#statementParser)
* [StreamedQueryResource](StreamedQueryResource.md#statementParser)
* [WSQueryEndpoint](WSQueryEndpoint.md#statementParser)

## Creating Instance

`StatementParser` takes the following to be created:

* <span id="ksqlEngine"> [KsqlExecutionContext](../KsqlExecutionContext.md)

`StatementParser` is created when:

* `KsqlRestApplication` is requested to [startAsync](KsqlRestApplication.md#startAsync) (and create a [WSQueryEndpoint](WSQueryEndpoint.md#statementParser))
* `InteractiveStatementExecutor` is [created](InteractiveStatementExecutor.md#statementParser)
* `StreamedQueryResource` is [created](StreamedQueryResource.md#statementParser)

## <span id="parseSingleStatement"> Parsing KSQL Statement

```java
<T extends Statement> PreparedStatement<T> parseSingleStatement(
  String statementString)
```

`parseSingleStatement` requests the [ksqlEngine](#ksqlEngine) to [parse the KSQL statement](../KsqlExecutionContext.md#parse) and then to [prepare it](../KsqlExecutionContext.md#prepare).

`parseSingleStatement` throws an `IllegalArgumentException` if there are more KSQL statements given:

```text
Expected exactly one KSQL statement; found [size] instead
```

---

`parseSingleStatement` is used when:

* `InteractiveStatementExecutor` is requested to [handleStatementWithTerminatedQueries](InteractiveStatementExecutor.md#handleStatementWithTerminatedQueries)
* `StreamedQueryResource` is requested to [parse a KSQL statement](StreamedQueryResource.md#parseStatement)
* `WSQueryEndpoint` is requested to [parse a KSQL statement](WSQueryEndpoint.md#parseStatement))
