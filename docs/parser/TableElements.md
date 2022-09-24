# TableElements

## Creating Instance

`TableElements` takes the following to be created:

* <span id="elements"> `TableElement`s

`TableElements` is created using [of](#of) factory.

## <span id="of"> Creating TableElements

```java
TableElements of(
  TableElement... elements)
TableElements of(
  List<TableElement> elements)
```

`of` creates a [TableElements](#creating-instance) with the `TableElement`s.

---

`of` is used when:

* `AstBuilder.Visitor` is requested to parse [CREATE TABLE](AstBuilder.Visitor.md#visitCreateTable), [CREATE STREAM](AstBuilder.Visitor.md#visitCreateStream), [ASSERT STREAM](AstBuilder.Visitor.md#visitAssertStream), [ASSERT TABLE](AstBuilder.Visitor.md#visitAssertTable) statements
* `SchemaParser` is requested to parse a schema (in text format)
* `StatementRewriter.Rewriter` is requested to [visitCreateStream](../StatementRewriter.Rewriter.md#visitCreateStream), [visitCreateTable](../StatementRewriter.Rewriter.md#visitCreateTable)
* `DefaultSchemaInjector` is requested to [buildElements](../DefaultSchemaInjector.md#buildElements)

## <span id="toLogicalSchema"> toLogicalSchema

```java
LogicalSchema toLogicalSchema()
```

`toLogicalSchema`...FIXME

---

`toLogicalSchema` is used when:

* `CreateSourceFactory` is requested to [build a schema](../CreateSourceFactory.md#buildSchema)
* `SchemaRegisterInjector` is requested to [registerForCreateSource](../SchemaRegisterInjector.md#registerForCreateSource)
* `ColumnDeserializor` is requested to `deserialize`
* `LogicalSchemaDeserializer` is requested to `deserialize`
