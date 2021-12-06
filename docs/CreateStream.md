# CreateStream

`CreateStream` is a `CreateSource` and a `ExecutableDdlStatement` that represents the following SQL statements:

```text
CREATE (OR REPLACE)? (SOURCE)? STREAM (IF NOT EXISTS)? sourceName
  (tableElements)?
  (WITH tableProperties)?
```

```text
ASSERT STREAM sourceName (tableElements)? (WITH tableProperties)?
```

## Creating Instance

`CreateStream` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="name"> `SourceName`
* <span id="elements"> `TableElements`
* <span id="orReplace"> `orReplace` flag
* <span id="notExists"> `notExists` flag
* <span id="properties"> `CreateSourceProperties`
* <span id="isSource"> `isSource` flag

`CreateStream` is created when:

* `AstBuilder.Visitor` is requested to [visitCreateStream](AstBuilder.md#visitCreateStream) and [visitAssertStream](AstBuilder.md#visitAssertStream)
