# SqlTypeParser

## Demo

```scala
import io.confluent.ksql.schema.ksql.SqlTypeParser
val typeParser = SqlTypeParser.create(TypeRegistry.EMPTY)
```

## Creating Instance

`SqlTypeParser` takes the following to be created:

* <span id="typeRegistry"> TypeRegistry

`SqlTypeParser` is created using [create](#create) factory.

## <span id="create"> Creating SqlTypeParser

```java
SqlTypeParser create(
  TypeRegistry typeRegistry)
```

`create` creates a [SqlTypeParser](#creating-instance) with the given `TypeRegistry`.

---

`create` is used when:

* `UserFunctionLoader` is created
* `AstBuilder.Visitor` is [created](parser/AstBuilder.md#Visitor)
* `SchemaParser` is requested to `parse` a schema
* `SqlTypeDeserializer` is requested to `deserialize`
* `KsqlTargetUtil` is requested to `createSchema`

## <span id="getType"> getType

```java
Type getType(
  SqlBaseParser.TypeContext type)
```

`getType` [getSqlType](#getSqlType) and creates a `Type`.

---

`getType` is used when:

* `AstBuilder.Visitor` is requested to [visitAlterOption](parser/AstBuilder.Visitor.md#visitAlterOption), [visitCast](parser/AstBuilder.Visitor.md#visitCast), [visitTableElement](parser/AstBuilder.Visitor.md#visitTableElement), [visitRegisterType](parser/AstBuilder.Visitor.md#visitRegisterType)
* `SchemaParser` is requested to parse a schema
* `SqlTypeParser` is requested to [parse a schema](#parse)

### <span id="getSqlType"> getSqlType

```java
SqlType getSqlType(
  SqlBaseParser.TypeContext type)
```

`getSqlType`...FIXME
