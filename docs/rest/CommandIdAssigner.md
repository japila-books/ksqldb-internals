# CommandIdAssigner

`CommandIdAssigner` is used by [DistributingExecutor](DistributingExecutor.md#commandIdAssigner) to [execute KSQL statements](DistributingExecutor.md#execute).

## <span id="getCommandId"> Generating CommandId (for Statement)

```java
CommandId getCommandId(
  Statement command)
```

`getCommandId` looks up the `CommandIdSupplier` for the given [Statement](../parser/Statement.md) in the [SUPPLIERS](#SUPPLIERS) registry and executes it (with the `command` statement).

`getCommandId` throws a `RuntimeException` when a `CommandIdSupplier` could not be found:

```text
Cannot assign command ID to statement of type [commandClassName]
```

---

`getCommandId` is used when:

* `DistributingExecutor` is requested to [execute a KSQL statement](DistributingExecutor.md#execute)

## <span id="SUPPLIERS"> CommandId Suppliers

`CommandIdAssigner` defines `SUPPLIERS` registry of [Statement](../parser/Statement.md)s and their `CommandId` suppliers (_functions_ that produce a `CommandId`).

The registry is used to [getCommandId](#getCommandId).

Statement | CommandIdSupplier
----------|------------------
 [InsertInto](../parser/InsertInto.md) | [getInsertIntoCommandId](#getInsertIntoCommandId)
 _others_ |

### <span id="getInsertIntoCommandId"> getInsertIntoCommandId

```java
CommandId getInsertIntoCommandId(
  InsertInto insertInto)
```

`getInsertIntoCommandId` creates a `CommandId`.

Type | Entity | Action
-----|--------|--------
 `STREAM` | [Target DataSource](../parser/InsertInto.md#getTarget) | `CREATE`

---

`getInsertIntoCommandId` is used when:

* `CommandIdAssigner` is requested for the [CommandId supplier](#SUPPLIERS) of [InsertInto](../parser/InsertInto.md) statements
