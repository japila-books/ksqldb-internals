# Sink

`Sink` represents the output (`sourceName`) of the following KSQL statements:

* [CREATE STREAM AS SELECT](AstBuilder_Visitor.md#visitCreateStreamAs)
* [CREATE TABLE AS SELECT](AstBuilder_Visitor.md#visitCreateTableAs)
* [INSERT INTO](AstBuilder_Visitor.md#visitInsertInto)

!!! note "Naming Convention"
    The part of these statements to denote the output name of a sink to write results to is called `sourceName` in ANTLR definition file.

    That'd be fairly confusing to use _source_ to mean _sink_, but the reason to call it `sourceName` is that in the end a sink becomes a [DataSource](../DataSource.md) other statements can read from. So it really makes sense to call it this way (even risking some confusion).
    
    This should clear up the confusion, _shouldn't it?_

## Creating Instance

`Sink` takes the following to be created:

* <span id="name"> Source Name
* <span id="createSink"> `createSink` flag
* <span id="replaces"> `replaces` flag
* <span id="properties"> [CreateSourceAsProperties](CreateSourceAsProperties.md)

`Sink` is created using [Sink.of](#of) utility.

## <span id="of"> Creating Sink Instance

```java
Sink of(
  SourceName name,
  boolean createSink,
  boolean replaces,
  CreateSourceAsProperties properties)
```

`of` creates a [Sink](#creating-instance).

!!! note
    `of` simply "replaces" `new` keyword to instantiate a `Sink` object to make code more fluent.

    === "new"

        ```java
        Sink s = new Sink(...)
        ```

    === "of"

        ```java
        Sink s = Sink.of(...)
        ```

---

`of` is used when:

* `CreateAsSelect` is requested for a [Sink](CreateAsSelect.md#getSink)
* `InsertInto` is requested to for a [Sink](InsertInto.md#getSink)
