# CreateSourceProperties

## Creating Instance

`CreateSourceProperties` takes the following to be created:

* <span id="originals"> Originals (`Map<String, Literal>`)
* <span id="durationParser"> Duration Parser (`Function<String, Duration>`)

`CreateSourceProperties` is created using [from](#from) factory and when:

* [withKeyValueSchemaName](#withKeyValueSchemaName)
* [withPartitions](#withPartitions)
* [withFormats](#withFormats)

While being created, `CreateSourceProperties` creates a `PropertiesConfig` and...FIXME

## <span id="from"> Creating CreateSourceProperties

```java
CreateSourceProperties from(
  Map<String, Literal> literals)
```

`from` creates a [CreateSourceProperties](#creating-instance) (with the given `literals` and the default `DurationParser`).

`from` is used when:

* `AstBuilder.Visitor` is requested to [visitCreateTable](AstBuilder.md#visitCreateTable), [visitCreateStream](AstBuilder.md#visitCreateStream), [visitAssertStream](AstBuilder.md#visitAssertStream) and [visitAssertTable](AstBuilder.md#visitAssertTable)
