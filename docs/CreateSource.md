# CreateSource

`CreateSource` is an [extension](#contract) of the [Statement](Statement.md) abstraction for [CREATE SOURCE](#implementations) statements.

## Contract

### <span id="copyWith"> copyWith

```java
CreateSource copyWith(
  TableElements elements,
  CreateSourceProperties properties)
```

### <span id="getSourceType"> getSourceType

```java
SourceType getSourceType()
```

## Implementations

* [CreateStream](CreateStream.md)
* `CreateTable`

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
