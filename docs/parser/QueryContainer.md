# QueryContainer

`QueryContainer` is an [abstraction](#contract) of [SQL statements](#implementations) with a [query](#getQuery).

## Contract

### <span id="getQuery"> getQuery

```java
Query getQuery()
```

[Query](Query.md)

Used when:

* `EngineExecutor` is requested to [plan a ConfiguredStatement](../EngineExecutor.md#plan)
* `TopicCreateInjector` is requested to `injectForCreateAsSelect`
* `DefaultTraversalVisitor` is requested to [visitInsertInto](DefaultTraversalVisitor.md#visitInsertInto), [visitCreateStreamAsSelect](DefaultTraversalVisitor.md#visitCreateStreamAsSelect), [visitCreateTableAsSelect](DefaultTraversalVisitor.md#visitCreateTableAsSelect)
* _a few others_

### <span id="getQueryId"> getQueryId

```java
Optional<String> getQueryId()
```

Used when:

* `EngineExecutor` is requested to [plan a ConfiguredStatement](../EngineExecutor.md#plan)

### <span id="getSink"> getSink

```java
Sink getSink()
```

Used when:

* `EngineExecutor` is requested to [plan a statement](../EngineExecutor.md#plan)

## Implementations

* [CreateAsSelect](CreateAsSelect.md)
* [InsertInto](InsertInto.md)
