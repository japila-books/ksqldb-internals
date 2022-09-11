# AlterSourceFactory

## Creating Instance

`AlterSourceFactory` takes the following to be created:

* <span id="metaStore"> [MetaStore](MetaStore.md)

`AlterSourceFactory` is created along with [CommandFactories](CommandFactories.md#alterSourceFactory).

## <span id="create"> Creating AlterSourceCommand

```java
AlterSourceCommand create(
  AlterSource statement)
```

`create` creates an [AlterSourceCommand](AlterSourceCommand.md) for the given [AlterSource](parser/AlterSource.md) statement.

---

`create` is used when:

* `CommandFactories` is requested to [handle ALTER SOURCE statement](CommandFactories.md#handleAlterSource)
