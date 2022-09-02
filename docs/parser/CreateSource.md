# CreateSource

`CreateSource` is an [extension](#contract) of the [Statement](Statement.md) abstraction for [CREATE SOURCE](#implementations) statements.

## Contract

### <span id="copyWith"> copyWith

```java
CreateSource copyWith(
  TableElements elements,
  CreateSourceProperties properties)
```

Overrides `TableElements` and `CreateSourceProperties` of this `CreateSource`

Used when:

* `StatementRewriter.Rewriter` is requested to `visitCreateStream` and `visitCreateTable`
* `DefaultFormatInjector` is requested to `injectForCreateStatement`
* `DefaultSchemaInjector` is requested to `addSchemaFields`
* `SourcePropertyInjector` is requested to `buildConfiguredStatement`
* `TopicCreateInjector` is requested to `injectForCreateSource`

### <span id="getSourceType"> getSourceType

```java
SourceType getSourceType()
```

## Implementations

* [CreateStream](CreateStream.md)
* [CreateTable](CreateTable.md)

## Creating Instance

`CreateSource` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="name"> `SourceName`
* <span id="elements"> `TableElements`
* <span id="orReplace"> `orReplace` flag
* <span id="notExists"> `notExists` flag
* <span id="properties"> [CreateSourceProperties](CreateSourceProperties.md)
* <span id="isSource"> `isSource` flag

!!! note "Abstract Class"
    `CreateSource` is an abstract class and cannot be created directly. It is created indirectly for the [concrete CreateSources](#implementations).
