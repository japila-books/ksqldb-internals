# Command

## Creating Instance

`Command` takes the following to be created:

* <span id="statement"> Statement Text
* <span id="overwriteProperties"> Overwrite Properties (`Map<String, Object>`)
* <span id="originalProperties"> Original Properties (`Map<String, String>`)
* <span id="plan"> [KsqlPlan](../KsqlPlan.md)
* <span id="version"> Version
* <span id="expectedVersion"> Expected Version

`Command` is created using [of](#of) utilities and when:

* `RestoreCommandsCompactor` is requested to `compact`

## <span id="of"> Creating Command

```java
Command of(
  ConfiguredKsqlPlan configuredPlan)
Command of(
  ConfiguredStatement<?> configuredStatement)
```

`of` creates a [Command](#creating-instance).

`of` is used when:

* `ValidatedCommandFactory` utility is used to [createForPlannedQuery](ValidatedCommandFactory.md#createForPlannedQuery), [createCommand](ValidatedCommandFactory.md#createCommand), [createForAlterSystemQuery](ValidatedCommandFactory.md#createForAlterSystemQuery), [createForTerminateQuery](ValidatedCommandFactory.md#createForTerminateQuery)
