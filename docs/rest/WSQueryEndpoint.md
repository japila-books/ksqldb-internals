# WSQueryEndpoint

## Creating Instance

`WSQueryEndpoint` takes the following to be created:

* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* [StatementParser](#statementParser)
* [KsqlEngine](#ksqlEngine)
* <span id="commandQueue"> [CommandQueue](CommandQueue.md)
* <span id="exec"> `ListeningScheduledExecutorService`
* <span id="activenessRegistrar"> `ActivenessRegistrar`
* <span id="commandQueueCatchupTimeout"> [ksql.server.command.response.timeout.ms](KsqlRestConfig.md#ksql.server.command.response.timeout.ms)
* <span id="authorizationValidator"> `KsqlAuthorizationValidator`
* <span id="errorHandler"> `Errors`
* <span id="denyListPropertyValidator"> `DenyListPropertyValidator`
* <span id="queryExecutor"> [QueryExecutor](QueryExecutor.md)

`WSQueryEndpoint` is created when:

* `KsqlRestApplication` is requested to [start](KsqlRestApplication.md#startAsync) (and initialize [wsQueryEndpoint](KsqlRestApplication.md#wsQueryEndpoint))

### <span id="statementParser"> StatementParser

`WSQueryEndpoint` is given a [StatementParser](StatementParser.md) when [created](#creating-instance).

The `StatementParser` is used in [parseStatement](#parseStatement) (to [parse a KSQL statement](StatementParser.md#parseSingleStatement)).

### <span id="ksqlEngine"> KsqlEngine

`WSQueryEndpoint` is given a [KsqlEngine](../KsqlEngine.md) when [created](#creating-instance).

The `KsqlEngine` is used to [executeStreamQuery](#executeStreamQuery) (to [access the MetaStore](../KsqlEngine.md#getMetaStore)) for authorization (only when [KsqlAuthorizationValidator](#authorizationValidator) is given).

## <span id="executeStreamQuery"> executeStreamQuery

```java
void executeStreamQuery(
  ServerWebSocket webSocket,
  MultiMap requestParams,
  KsqlSecurityContext ksqlSecurityContext,
  Context context,
  Optional<Long> timeout)
```

`executeStreamQuery`...FIXME

---

`executeStreamQuery` is used when:

* `KsqlServerEndpoints` is requested to [executeWebsocketStream](KsqlServerEndpoints.md#executeWebsocketStream)
