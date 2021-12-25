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

`create` is used when:

* `UserFunctionLoader` is created
* `AstBuilder.Visitor` is [created](parser/AstBuilder.md#Visitor)
* `SchemaParser` is requested to `parse` a schema
* `SqlTypeDeserializer` is requested to `deserialize`
* `KsqlTargetUtil` is requested to `createSchema`
