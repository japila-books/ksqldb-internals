# SqlPrimitiveType

`SqlPrimitiveType` is a [SqlType](SqlType.md).

## <span id="PRIMITIVE_TYPE_NAMES"> Primitive Types

* `ARRAY`
* `BIGINT`
* `BOOLEAN`
* `BYTES`
* `DATE`
* `DECIMAL`
* `DOUBLE`
* `INTEGER`
* `INT`
* `MAP`
* `STRING`
* `STRUCT`
* `TIMESTAMP`
* `TIME`
* `VARCHAR`

## Demo

```scala
import io.confluent.ksql.schema.ksql.types._
val string = SqlPrimitiveType.of(SqlBaseType.STRING)
assert(string.toString == "STRING")
```
