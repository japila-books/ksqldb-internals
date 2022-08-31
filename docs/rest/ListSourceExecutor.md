# ListSourceExecutor

`ListSourceExecutor` is a [CustomExecutors](CustomExecutors.md) to handle the following statements:

* [LIST_STREAMS](#streams)
* _others_

## <span id="streams"> streams

```scala
StatementExecutorResponse streams(
  ConfiguredStatement<ListStreams> statement,
  SessionProperties sessionProperties,
  KsqlExecutionContext executionContext,
  ServiceContext serviceContext)
```

`streams` [getSpecificStreams](#getSpecificStreams).

`streams`...FIXME

---

`streams` is used when:

* `CustomExecutors` is requested for the [LIST_STREAMS](CustomExecutors.md#LIST_STREAMS) enum
