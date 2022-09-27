# DelimitedFormat

`DelimitedFormat` is a [Format](Format.md) known as [DELIMITED](#NAME).

## <span id="name"><span id="NAME"> Name

```java
String name()
```

`name` is part of the [Format](Format.md#name) abstraction.

---

`name` is `DELIMITED`.

## <span id="getSupportedProperties"><span id="DELIMITER"> Properties

```java
Set<String> getSupportedProperties()
```

`getSupportedProperties` is part of the [Format](Format.md#getSupportedProperties) abstraction.

---

`getSupportedProperties` is `delimiter`.

## <span id="supportsKeyType"> Key Types

```java
boolean supportsKeyType(
  SqlType type)
```

`supportsKeyType` is part of the [Format](Format.md#supportsKeyType) abstraction.

---

`supportsKeyType` is `true` for [SqlPrimitiveType](../types/SqlPrimitiveType.md)s.
