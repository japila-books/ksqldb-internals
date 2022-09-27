# SqlPrimitiveType

`SqlPrimitiveType` is a [SqlType](SqlType.md) and represents the following primitive types:

* `BIGINT`
* `BOOLEAN`
* `BYTES`
* `DATE`
* `DOUBLE`
* `INT` (a synonym of `INTEGER`)
* `INTEGER`
* `STRING`
* `TIME`
* `TIMESTAMP`
* `VARCHAR` (a synonym of `STRING`)

## Demo

```scala
import io.confluent.ksql.schema.ksql.types._
val string = SqlPrimitiveType.of(SqlBaseType.STRING)
assert(string.toString == "STRING")
```
