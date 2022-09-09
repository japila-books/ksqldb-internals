# KsqlPlan

`KsqlPlan` is an [abstraction](#contract) of [query plans](#implementations).

## Contract

### <span id="getDdlCommand"> getDdlCommand

```java
Optional<DdlCommand> getDdlCommand()
```

[DdlCommand](DdlCommand.md)

Used when:

* `EngineExecutor` is requested to [execute a query](EngineExecutor.md#execute)
* `DefaultSchemaInjector` is requested to `forCreateAsStatement`
* `SchemaRegisterInjector` is requested to [registerForCreateAs](SchemaRegisterInjector.md#registerForCreateAs)
* `RestoreCommandsCompactor` is requested to `compact`
* `RestoreCommandsCompactor.CompactedNode` is requested to `maybeAppend`

### <span id="getPersistentQueryType"> getPersistentQueryType

```java
Optional<KsqlConstants.PersistentQueryType> getPersistentQueryType()
```

`PersistentQueryType` that can be one of the following:

* `CREATE_SOURCE`
* `CREATE_AS`
* `INSERT`

Used when:

* `EngineExecutor` is requested to [execute a query](EngineExecutor.md#execute)

### <span id="getQueryPlan"> getQueryPlan

```java
Optional<QueryPlan> getQueryPlan()
```

[QueryPlan](QueryPlan.md)

Used when:

* `EngineExecutor` is requested to [execute a query](EngineExecutor.md#execute)
* `RestoreCommandsCompactor.CompactedNode` is requested to `maybeAppend`

### <span id="getStatementText"> getStatementText

```java
String getStatementText()
```

[Statement](parser/Statement.md) (in text format)

Used when:

* `EngineExecutor` is requested to [execute a query](EngineExecutor.md#execute)
* `KsqlEngine` is requested to [execute a statement](KsqlEngine.md#execute)
* `Command` utility is used to [create a Command](rest/Command.md#of)

### <span id="withoutQuery"> withoutQuery

```java
KsqlPlan withoutQuery()
```

Used when:

* `RestoreCommandsCompactor` is requested to `compact`

## Implementations

* [KsqlPlanV1](KsqlPlanV1.md)

## <span id="ddlPlanCurrent"> Creating KsqlPlan for DdlCommand (ddlPlanCurrent)

```java
KsqlPlan ddlPlanCurrent(
  String statementText,
  DdlCommand ddlCommand)
```

`ddlPlanCurrent` creates a [KsqlPlanV1](KsqlPlanV1.md) with the given `statementText` and the [DdlCommand](DdlCommand.md).

`ddlPlanCurrent` is used when:

* `EngineExecutor` is requested to [plan an ExecutableDdlStatement](EngineExecutor.md#plan-ExecutableDdlStatement) (for a non-source table)

## <span id="queryPlanCurrent"> Creating KsqlPlan for QueryPlan (queryPlanCurrent)

```java
KsqlPlan queryPlanCurrent(
  String statementText,
  Optional<DdlCommand> ddlCommand,
  QueryPlan queryPlan)
```

`queryPlanCurrent` creates a [KsqlPlanV1](KsqlPlanV1.md).

`queryPlanCurrent` is used when:

* `EngineExecutor` is requested to [plan an ExecutableDdlStatement](EngineExecutor.md#plan-ExecutableDdlStatement) (with a [source table](EngineExecutor.md#sourceTablePlan)) and [plan a QueryContainer](EngineExecutor.md#plan-QueryContainer)
