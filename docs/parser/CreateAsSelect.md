# CreateAsSelect

`CreateAsSelect` is an [extension](#contract) of [Statement](Statement.md) and [QueryContainer](QueryContainer.md) abstractions for [CREATE AS SELECT](#implementations) statements.

## Contract

### <span id="copyWith"> copyWith

```java
CreateAsSelect copyWith(
  CreateSourceAsProperties properties)
```

Used when:

* `DefaultSchemaInjector` is requested to [addSchemaFieldsCas](../DefaultSchemaInjector.md#addSchemaFieldsCas)
* `SourcePropertyInjector` is requested to `buildConfiguredStatement`
* `TopicCreateInjector` is requested to `injectForCreateAsSelect`

## Implementations

* [CreateStreamAsSelect](CreateStreamAsSelect.md)
* [CreateTableAsSelect](CreateTableAsSelect.md)

## Creating Instance

`CreateAsSelect` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="name"> Source Name
* <span id="query"> [Query](Query.md)
* <span id="orReplace"> `orReplace` flag
* <span id="notExists"> `notExists` flag
* <span id="properties"> [CreateSourceAsProperties](CreateSourceAsProperties.md)

!!! note "Abstract Class"
    `CreateAsSelect` is an abstract class and cannot be created directly. It is created indirectly for the [concrete CreateAsSelects](#implementations).

## <span id="getSink"> getSink

```java
Sink getSink()
```

`getSink` is part of the [QueryContainer](QueryContainer.md#getSink) abstraction.

---

`getSink` [creates a Sink](Sink.md#of) (with [createSink](Sink.md#createSink) flag enabled).
