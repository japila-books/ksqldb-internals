# KsqlPlanV1

`KsqlPlanV1` is a [KsqlPlan](KsqlPlan.md).

## Creating Instance

`KsqlPlanV1` takes the following to be created:

* <span id="statementText"> Statement Text
* <span id="ddlCommand"> DDL command
* <span id="queryPlan"> [QueryPlan](QueryPlan.md)

When created, `KsqlPlanV1` makes sure that either a [DDL command](#ddlCommand) or [query plan](#queryPlan) is given.

`KsqlPlanV1` is created when:

* `KsqlPlan` is requested for [ddlPlanCurrent](KsqlPlan.md#ddlPlanCurrent) and [queryPlanCurrent](KsqlPlan.md#queryPlanCurrent)
* `KsqlPlanV1` is requested to [create a new KsqlPlanV1 without a query plan](#withoutQuery)
