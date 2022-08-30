# ConfiguredKsqlPlan

`ConfiguredKsqlPlan` is a [KsqlPlan](#plan) with a [SessionConfig](#config).

## Creating Instance

`ConfiguredKsqlPlan` takes the following to be created:

* <span id="plan"> [KsqlPlan](KsqlPlan.md)
* <span id="config"> [SessionConfig](SessionConfig.md)

`ConfiguredKsqlPlan` is created using [of](#of) utility.

## <span id="of"> of

```java
ConfiguredKsqlPlan of(
  KsqlPlan plan,
  SessionConfig config
)
```

`of` creates a [ConfiguredKsqlPlan](#creating-instance).

---

`of` is used when:

* `KsqlEngine` is requested to [execute a statement](KsqlEngine.md#execute)
* `SandboxedExecutionContext` is requested to [execute a statement](SandboxedExecutionContext.md#execute)
* `InteractiveStatementExecutor` is requested to [execute a KsqlPlan](rest/InteractiveStatementExecutor.md#execute)
* `ValidatedCommandFactory` is requested to [createForPlannedQuery](rest/ValidatedCommandFactory.md#createForPlannedQuery)
