# CommandStoreUtil

## <span id="httpWaitForCommandSequenceNumber"> httpWaitForCommandSequenceNumber

```java
void httpWaitForCommandSequenceNumber(
  CommandQueue commandQueue,
  KsqlRequest request,
  Duration timeout)
```

`httpWaitForCommandSequenceNumber`...FIXME

---

`httpWaitForCommandSequenceNumber` is used when:

* `KsqlResource` is requested to [handleKsqlStatements](KsqlResource.md#handleKsqlStatements)
* `StreamedQueryResource` is requested to [streamQuery](StreamedQueryResource.md#streamQuery).

## <span id="waitForCommandSequenceNumber"> waitForCommandSequenceNumber

```java
void waitForCommandSequenceNumber(
  CommandQueue commandQueue,
  KsqlRequest request,
  Duration timeout)
```

`waitForCommandSequenceNumber` requests the given `KsqlRequest` for `getCommandSequenceNumber` and, only if the sequence number is specified, requests the given [CommandQueue](CommandQueue.md) to [ensureConsumedPast](CommandQueue.md#ensureConsumedPast).

---

`waitForCommandSequenceNumber` is used when:

* `WSQueryEndpoint` is requested to [executeStreamQuery](WSQueryEndpoint.md#executeStreamQuery)
* `CommandStoreUtil` is requested to [httpWaitForCommandSequenceNumber](#httpWaitForCommandSequenceNumber).
