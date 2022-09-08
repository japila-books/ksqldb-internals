# SequenceNumberFutureStore

## Creating Instance

`SequenceNumberFutureStore` takes no parameters to be created.

`SequenceNumberFutureStore` initializes the [sequenceNumberFutures](#sequenceNumberFutures) and the [lastCompletedSequenceNumber](#lastCompletedSequenceNumber).

`SequenceNumberFutureStore` is created when:

* `CommandStore.Factory` is requested to [create a CommandStore](CommandStore.md#create) (for [sequenceNumberFutureStore](CommandStore.md#sequenceNumberFutureStore))

## <span id="getFutureForSequenceNumber"> getFutureForSequenceNumber

```java
CompletableFuture<Void> getFutureForSequenceNumber(
  long seqNum)
```

For the given `seqNum` less than the [lastCompletedSequenceNumber](#lastCompletedSequenceNumber), `getFutureForSequenceNumber` returns a new `CompletableFuture` that is already completed with `null` value.

Otherwise, `getFutureForSequenceNumber` requests the [sequenceNumberFutures](#sequenceNumberFutures) for the `CompletableFuture` for the given `seqNum` (if available) or creates a new one.

---

`getFutureForSequenceNumber` is used when:

* `CommandStore` is requested to [ensureConsumedPast](CommandStore.md#ensureConsumedPast)

## <span id="completeFuturesUpToAndIncludingSequenceNumber"> completeFuturesUpToAndIncludingSequenceNumber

```java
void completeFuturesUpToAndIncludingSequenceNumber(
  long seqNum)
```

`completeFuturesUpToAndIncludingSequenceNumber` sets the [lastCompletedSequenceNumber](#lastCompletedSequenceNumber) to be the given `seqNum`.

`completeFuturesUpToAndIncludingSequenceNumber` completes and removes all `CompletableFuture`s up to and including the given `seqNum`.

---

`completeFuturesUpToAndIncludingSequenceNumber` is used when:

* `CommandStore` is requested to [completeSatisfiedSequenceNumberFutures](CommandStore.md#completeSatisfiedSequenceNumberFutures)
