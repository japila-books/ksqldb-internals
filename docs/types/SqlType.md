# SqlType

`SqlType` is an [abstraction](#contract) of [SQL types](#implementations).

## Contract

### <span id="toString"> toString

```java
String toString(
  FormatOptions formatOptions)
```

Used when:

* `Column` is requested to [toString](../udf/Column.md#toString)
* `SqlTypeSchemaSerializer` is requested to [serialize a SqlType](../udf/SqlTypeSchemaSerializer.md#serialize)
* `FilterTypeValidator` is requested to `validateFilterExpression`
* `ExpressionFormatter.Formatter` is requested to `visitType`
* `QueryEndpoint` is requested to [colTypesFromSchema](../rest/QueryEndpoint.md#colTypesFromSchema)

## Implementations

* `SqlArray`
* `SqlDecimal`
* `SqlMap`
* [SqlPrimitiveType](SqlPrimitiveType.md)
* `SqlStruct`

## Creating Instance

`SqlType` takes the following to be created:

* <span id="baseType"> [SqlBaseType](SqlBaseType.md)

!!! note "Abstract Class"
    `SqlType` is an abstract class and cannot be created directly. It is created indirectly for the [concrete SqlTypes](#implementations).

## <span id="SqlTypeSchemaSerializer"> SqlTypeSchemaSerializer

`SqlType` is serialized using [SqlTypeSchemaSerializer](../udf/SqlTypeSchemaSerializer.md).
