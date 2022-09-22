# InsertInto

`InsertInto` is a [Statement](Statement.md) and a [QueryContainer](QueryContainer.md) that represents [INSERT INTO](AstBuilder.Visitor.md#visitInsertInto) statement.

!!! note
    `INSERT INTO` can only be used to [insert into a stream](../AstSanitizer.RewriterPlugin.md#visitInsertInto).

## Creating Instance

`InsertInto` takes the following to be created:

* <span id="location"> `NodeLocation`
* [Target DataSource](#target)
* <span id="query"> [Query](Query.md)
* <span id="properties"> `InsertIntoProperties`

`InsertInto` is created when:

* `AstBuilder.Visitor` is requested to [parse INSERT INTO statement](AstBuilder.Visitor.md#visitInsertInto)
* `StatementRewriter.Rewriter` is requested to [visitInsertInto](../StatementRewriter.Rewriter.md#visitInsertInto)

### <span id="target"><span id="getTarget"> Target DataSource

`InsertInto` is given the name of the target [DataSource](../DataSource.md) (to write records into). The target should already be in [MetaStore](../MetaStore.md).

#### getTarget

```java
SourceName getTarget()
```

`getTarget` returns the [target](#target).

---

`getTarget` is used when:

* `AstSanitizer.RewriterPlugin` is requested to [visitInsertInto](../AstSanitizer.RewriterPlugin.md#visitInsertInto)
* `KsqlAuthorizationValidatorImpl` is requested to [validateInsertInto](../KsqlAuthorizationValidatorImpl.md#validateInsertInto)
* `CommandIdAssigner` is requested to [getInsertIntoCommandId](../rest/CommandIdAssigner.md#getInsertIntoCommandId)
* `DistributingExecutor` is requested to [validateInsertIntoQueries](../rest/DistributingExecutor.md#validateInsertIntoQueries)

## <span id="getSink"> getSink

```java
Sink getSink()
```

`getSink` is part of the [QueryContainer](QueryContainer.md#getSink) abstraction.

---

`getSink` [creates a Sink](Sink.md#of) (with [createSink](Sink.md#createSink) flag disabled).
