# SqlBaseType

`SqlBaseType` is a collection (_enum_) of built-in SQL data types in ksqlDB:

* `ARRAY`
* `BIGINT`
* `BOOLEAN`
* `BYTES`
* `DATE`
* `DECIMAL`
* `DOUBLE`
* `INTEGER`
* `MAP`
* `STRING`
* `STRUCT`
* `TIME`
* `TIMESTAMP`

## <span id="isNumber"><span id="numbers"> Numeric Types

```java
boolean isNumber()
```

`isNumber` is `true` when this `SqlBaseType` is one of the following:

* `INTEGER`
* `BIGINT`
* `DECIMAL`
* `DOUBLE`

## <span id="isTime"> Time Types

```java
boolean isTime()
```

`isTime` is `true` when this `SqlBaseType` is one of the following:

* `TIME`
* `DATE`
* `TIMESTAMP`
