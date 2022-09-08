# WSQueryEndpoint

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
