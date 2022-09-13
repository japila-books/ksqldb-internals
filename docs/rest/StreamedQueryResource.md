# StreamedQueryResource

## Creating Instance

`StreamedQueryResource` takes the following to be created:

* <span id="ksqlEngine"> [KsqlExecutionContext](../KsqlExecutionContext.md)
* <span id="ksqlRestConfig"> [KsqlRestConfig](KsqlRestConfig.md)
* [StatementParser](#statementParser)
* [CommandQueue](#commandQueue)
* <span id="disconnectCheckInterval"> `disconnectCheckInterval`
* <span id="commandQueueCatchupTimeout"> `commandQueueCatchupTimeout`
* <span id="activenessRegistrar"> `ActivenessRegistrar`
* <span id="authorizationValidator"> `KsqlAuthorizationValidator`
* <span id="errorHandler"> `Errors` handler
* <span id="denyListPropertyValidator"> `DenyListPropertyValidator`
* <span id="queryExecutor"> [QueryExecutor](QueryExecutor.md)

`StreamedQueryResource` is created when:

* `KsqlRestApplication` is requested to [build a KsqlRestApplication](KsqlRestApplication.md#buildApplication)

### <span id="commandQueue"> CommandQueue

`StreamedQueryResource` is given a [CommandQueue](CommandQueue.md) when [created](#creating-instance).

The `CommandQueue` is used to [httpWaitForCommandSequenceNumber](CommandStoreUtil.md#httpWaitForCommandSequenceNumber) when [streamQuery](#streamQuery).

### <span id="statementParser"> StatementParser

`StreamedQueryResource` is given a [StatementParser](StatementParser.md) when [created](#creating-instance).

The `StatementParser` is used in [parseStatement](#parseStatement) (to [parse a KSQL statement](StatementParser.md#parseSingleStatement)).

## <span id="streamQuery"> streamQuery

```java
EndpointResponse streamQuery(
  KsqlSecurityContext securityContext,
  KsqlRequest request,
  CompletableFuture<Void> connectionClosedFuture,
  Optional<Boolean> isInternalRequest,
  MetricsCallbackHolder metricsCallbackHolder,
  Context context)
```

`streamQuery` requests the [ActivenessRegistrar](#activenessRegistrar) to `updateLastRequestTime`.

`streamQuery` [parseStatement](#parseStatement) the given `KsqlRequest` (into a `PreparedStatement`).

`streamQuery` [httpWaitForCommandSequenceNumber](CommandStoreUtil.md#httpWaitForCommandSequenceNumber) (with the [commandQueue](#commandQueue), the given `KsqlRequest`, and the [commandQueueCatchupTimeout](#commandQueueCatchupTimeout)).

In the end, `streamQuery` [handles the ksql statement](#handleStatement).

---

`streamQuery` is used when:

* `KsqlServerEndpoints` is requested to [executeQueryRequest](KsqlServerEndpoints.md#executeQueryRequest)

### <span id="parseStatement"> parseStatement

```java
PreparedStatement<?> parseStatement(
  KsqlRequest request)
```

`parseStatement` requests the [StatementParser](#statementParser) to [parse](StatementParser.md#parseSingleStatement) the ksql statement (from the given `KsqlRequest`).

### <span id="handleStatement"> handleStatement

```java
EndpointResponse handleStatement(
  KsqlSecurityContext securityContext,
  KsqlRequest request,
  PreparedStatement<?> statement,
  CompletableFuture<Void> connectionClosedFuture,
  Optional<Boolean> isInternalRequest,
  MetricsCallbackHolder metricsCallbackHolder,
  Context context)
```

`handleStatement` handles [Query](#handleStatement-Query) and [PrintTopic](#handleStatement-PrintTopic) statements only.

---

`handleStatement` requests the [KsqlAuthorizationValidator](#authorizationValidator) to `checkAuthorization` (if defined).

`handleStatement` requests the [DenyListPropertyValidator](#denyListPropertyValidator) to `validateAll` the config overrides (from the given `KsqlRequest`).

#### <span id="handleStatement-Query"> Query

For a [Query](../parser/Query.md) statement, `handleStatement` requests the [QueryExecutor](#queryExecutor) to [handle the statement](QueryExecutor.md#handleStatement) and then [handleQuery](#handleQuery).

#### <span id="handleStatement-PrintTopic"> PrintTopic

For a `PrintTopic` statement, `handleStatement` [handlePrintTopic](#handlePrintTopic).

### <span id="handleQuery"> handleQuery

```java
EndpointResponse handleQuery(
  PreparedStatement<Query> statement,
  CompletableFuture<Void> connectionClosedFuture,
  QueryMetadataHolder queryMetadataHolder)
```

`handleQuery` handles pull and push queries (and uses the given `QueryMetadataHolder` to determine the type).

For a pull query, `handleQuery` requests the given `QueryMetadataHolder` for the `PullQueryResult` and returns a new `PullQueryStreamWriter` in response.

For a push query, `handleQuery` requests the given `QueryMetadataHolder` for the `PushQueryMetadata` and returns a new `QueryStreamWriter` in response.

For other types, `handleQuery` responds with `400 Bad Request` error code and the following message:

```text
Statement type `className' not supported for this resource
```
