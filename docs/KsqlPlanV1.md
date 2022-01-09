# KsqlPlanV1

`KsqlPlanV1` is a [KsqlPlan](KsqlPlan.md) that `EngineExecutor` uses to hold the result of [planning statements](EngineExecutor.md#plan) (as a container for a [DdlCommand](#ddlCommand), a [QueryPlan](#queryPlan) or both):

1. [ExecutableDdlStatement](EngineExecutor.md#plan-ExecutableDdlStatement) with a non-source table (gives a [DdlCommand](#ddlCommand) only)
1. [ExecutableDdlStatement](EngineExecutor.md#plan-ExecutableDdlStatement) with a [source table](EngineExecutor.md#sourceTablePlan) (gives a [DdlCommand](#ddlCommand) and a [QueryPlan](#queryPlan) for changes)
1. [QueryContainer](EngineExecutor.md#plan-QueryContainer) (gives a [QueryPlan](#queryPlan) and perhaps a [DdlCommand](#ddlCommand) to [create a sink](EngineExecutor.md#maybeCreateSinkDdl))

## Creating Instance

`KsqlPlanV1` takes the following to be created:

* <span id="statementText"> Statement Text
* <span id="ddlCommand"> (optional) [DdlCommand](DdlCommand.md)
* <span id="queryPlan"> (optional) [QueryPlan](QueryPlan.md)

When created, `KsqlPlanV1` makes sure that either a [DdlCommand](#ddlCommand) or a [QueryPlan](#queryPlan) is given.

`KsqlPlanV1` is created when:

* `KsqlPlan` utility is used to create a `KsqlPlan` for a [DdlCommand](KsqlPlan.md#ddlPlanCurrent) or a [QueryPlan](KsqlPlan.md#queryPlanCurrent)
* `KsqlPlanV1` is requested for a [copy of the current KsqlPlanV1 without a query plan](#withoutQuery)

## <span id="withoutQuery"> Current KsqlPlanV1 Without QueryPlan (withoutQuery)

```java
KsqlPlan withoutQuery()
```

`withoutQuery` creates a new [KsqlPlanV1](#creating-instance) (with the [statementText](#statementText) and the [ddlCommand](#ddlCommand) but with no [QueryPlan](#queryPlan)).

`withoutQuery` is part of the [KsqlPlan](KsqlPlan.md#withoutQuery) abstraction.
