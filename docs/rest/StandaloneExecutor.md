# StandaloneExecutor

`StandaloneExecutor` is an [Executable](Executable.md).

## <span id="startAsync"> startAsync

```java
void startAsync()
```

`startAsync` requests the [UserFunctionLoader](#udfLoader) to [load](../UserFunctionLoader.md#load).

`startAsync` [maybeCreateProcessingLogTopic](ProcessingLogServerUtils.md#maybeCreateProcessingLogTopic).

With `stream.auto.create` enabled, `startAsync` prints out the following WARN message to the logs:

```text
processing log auto-create is enabled, but this is not supported for headless mode.
```

`startAsync` [reads](#readQueriesFile) the [queriesFile](#queriesFile) to [process](#processesQueryFile).

`startAsync` [showWelcomeMessage](#showWelcomeMessage).

In the end, `startAsync` requests the [VersionCheckerAgent](#versionChecker) to [start](../VersionCheckerAgent.md#start) (with the `SERVER` module type and non-null configuration properties of the [KsqlConfig](#ksqlConfig)).

---

`startAsync` is part of the [Executable](Executable.md#startAsync) abstraction.

### <span id="processesQueryFile"> processesQueryFile

```java
void processesQueryFile(
  String queries)
```

`processesQueryFile`...FIXME

### <span id="validateStatements"> Validating Statements

```java
void validateStatements(
  List<ParsedStatement> statements)
```

`validateStatements`...FIXME

## <span id="executeStatements"> Executing Statements

```java
boolean executeStatements(
  List<ParsedStatement> statements,
  StatementExecutor executor)
```

`executeStatements` requests the given [StatementExecutor](StatementExecutor.md) to [execute](StatementExecutor.md#execute) the given `ParsedStatement`s one by one.

In the end, `executeStatements` returns whether there was a `ParsedStatement` with a query.

`executeStatements` is used when:

* `StandaloneExecutor` is requested to [processes a query file](#processesQueryFile) and [validate statements](#validateStatements)
