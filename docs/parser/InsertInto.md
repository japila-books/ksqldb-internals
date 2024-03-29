# InsertInto

`InsertInto` is a [Statement](Statement.md) and a [QueryContainer](QueryContainer.md) that represents [INSERT INTO](AstBuilder.Visitor.md#visitInsertInto) statement.

`InsertInto` is a distributed command and executed by [DistributingExecutor](../rest/DistributingExecutor.md) (in [REST execution mode](../rest/index.md)).

## Requirements

1. `INSERT INTO` can only be used to [insert into a stream](../AstSanitizer.RewriterPlugin.md#visitInsertInto)
1. The [target](#target) should be [registered in MetaStore](../rest/DistributingExecutor.md#validateInsertIntoQueries)
1. The [target](#target) datasource [cannot have header columns](../rest/DistributingExecutor.md#validateInsertIntoQueries)
1. The topic of the [target](#target) datasource [cannot be read-only](../rest/DistributingExecutor.md#validateInsertIntoQueries)

!!! question "Possible Code Duplication"
    There are two entities that assert these requirements:

    * [AstSanitizer.RewriterPlugin](../AstSanitizer.RewriterPlugin.md#visitInsertInto)
    * [DistributingExecutor](../rest/DistributingExecutor.md#validateInsertIntoQueries)

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

`InsertInto` is given the name of the target [DataSource](../DataSource.md) (to write records into).

#### getTarget

```java
SourceName getTarget()
```

`getTarget` returns the [target](#target).

---

`getTarget` is used when:

* `AstSanitizer.RewriterPlugin` is requested to [visitInsertInto](../AstSanitizer.RewriterPlugin.md#visitInsertInto)
* `KsqlAuthorizationValidatorImpl` is requested to [validateInsertInto](../security/KsqlAuthorizationValidatorImpl.md#validateInsertInto)
* `CommandIdAssigner` is requested to [getInsertIntoCommandId](../rest/CommandIdAssigner.md#getInsertIntoCommandId)
* `DistributingExecutor` is requested to [validateInsertIntoQueries](../rest/DistributingExecutor.md#validateInsertIntoQueries)

## <span id="getSink"> getSink

```java
Sink getSink()
```

`getSink` is part of the [QueryContainer](QueryContainer.md#getSink) abstraction.

---

`getSink` [creates a Sink](Sink.md#of) (with [createSink](Sink.md#createSink) flag disabled).

## Learning Resources

1. [Insert Into | Level Up your KSQL by Confluent](https://youtu.be/z508VDdtp_M)
1. [KSQL demo - INSERT INTO](https://asciinema.org/a/191977)
