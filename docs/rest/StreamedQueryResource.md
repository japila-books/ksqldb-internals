# StreamedQueryResource

## Creating Instance

`StreamedQueryResource` takes the following to be created:

* <span id="ksqlEngine"> [KsqlExecutionContext](../KsqlExecutionContext.md)
* <span id="ksqlRestConfig"> [KsqlRestConfig](KsqlRestConfig.md)
* <span id="statementParser"> [StatementParser](StatementParser.md)
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

`streamQuery`...FIXME

---

`streamQuery` is used when:

* `KsqlServerEndpoints` is requested to [executeQueryRequest](KsqlServerEndpoints.md#executeQueryRequest)
