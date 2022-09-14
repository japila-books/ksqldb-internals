# QueryPublisher

`QueryPublisher` is an [extension](#contract) of the `Publisher` (Reactive Streams) of `KeyValueMetadata<List<?>`s and `GenericRow`s abstraction for [query result publishers](#implementations).

## Contract (Sunset)

### <span id="isPullQuery"> isPullQuery

```java
boolean isPullQuery()
```

See [BlockingQueryPublisher](BlockingQueryPublisher.md#isPullQuery)

Used when:

* `QueryStreamHandler` is requested to [handleQueryPublisher](QueryStreamHandler.md#handleQueryPublisher)

## Implementations

* [BlockingQueryPublisher](BlockingQueryPublisher.md)
