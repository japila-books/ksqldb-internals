# StandaloneExecutor

`StandaloneExecutor` is the [Executable](../rest/Executable.md) for [Headless Execution Mode](index.md).

## Creating Instance

`StandaloneExecutor` takes the following to be created:

* <span id="serviceContext"> [ServiceContext](../ServiceContext.md)
* <span id="processingLogConfig"> [ProcessingLogConfig](../monitoring/ProcessingLogConfig.md)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* [KsqlEngine](#ksqlEngine)
* <span id="queriesFile"> Query File
* <span id="udfLoader"> [UserFunctionLoader](../UserFunctionLoader.md)
* <span id="failOnNoQueries"> `failOnNoQueries` flag
* <span id="versionChecker"> [VersionCheckerAgent](../VersionCheckerAgent.md)
* <span id="injectorFactory"> Factory to build an [Injector](../Injector.md) (based on [KsqlExecutionContext](../KsqlExecutionContext.md) and [ServiceContext](../ServiceContext.md))
* <span id="metricCollectors"> [MetricCollectors](../metrics/MetricCollectors.md)

`StandaloneExecutor` is created when:

* `StandaloneExecutorFactory` utility is used to [create a StandaloneExecutor](StandaloneExecutorFactory.md#create)

### <span id="ksqlEngine"> KsqlEngine

`StandaloneExecutor` is given a [KsqlEngine](../KsqlEngine.md) when [created](#creating-instance).

The `KsqlEngine` is active until [shutdown](#shutdown).

The `KsqlEngine` is used when:

* [validateStatements](#validateStatements)
* [processesQueryFile](#processesQueryFile)

## <span id="startAsync"> startAsync

```java
void startAsync()
```

`startAsync` requests the [UserFunctionLoader](#udfLoader) to [load](../UserFunctionLoader.md#load).

`startAsync` [maybeCreateProcessingLogTopic](../rest/ProcessingLogServerUtils.md#maybeCreateProcessingLogTopic).

With `stream.auto.create` enabled, `startAsync` prints out the following WARN message to the logs:

```text
processing log auto-create is enabled, but this is not supported for headless mode.
```

`startAsync` [reads](#readQueriesFile) the [queriesFile](#queriesFile) to [process](#processesQueryFile).

`startAsync` [showWelcomeMessage](#showWelcomeMessage).

In the end, `startAsync` requests the [VersionCheckerAgent](#versionChecker) to [start](../VersionCheckerAgent.md#start) (with the `SERVER` module type and non-null configuration properties of the [KsqlConfig](#ksqlConfig)).

---

`startAsync` is part of the [Executable](../rest/Executable.md#startAsync) abstraction.

### <span id="readQueriesFile"> Loading Queries File

```java
String readQueriesFile(
  String queryFilePath)
```

`readQueriesFile` reads the given `queryFilePath` with `UTF_8` encoding.

### <span id="processesQueryFile"> Processing Queries

```java
void processesQueryFile(
  String queries)
```

`processesQueryFile` requests the [KsqlEngine](#ksqlEngine) to [parse the SQL queries](../KsqlEngine.md#parse) (into a collection of `ParsedStatement`s).

`processesQueryFile` [validates the ParsedStatements](#validateStatements).

`processesQueryFile` uses the [injectorFactory](#injectorFactory) to create an `Injector` (with the [KsqlEngine](#ksqlEngine) and the [ServiceContext](#serviceContext)).

`processesQueryFile`...FIXME

### <span id="validateStatements"> Validating Statements

```java
void validateStatements(
  List<ParsedStatement> statements)
```

`validateStatements` requests the [KsqlEngine](#ksqlEngine) to [create a SandboxedExecutionContext](../KsqlEngine.md#createSandbox) (with the [ServiceContext](#serviceContext)).

`validateStatements` uses the [injectorFactory](#injectorFactory) to create an `Injector` (with the [SandboxedExecutionContext](../SandboxedExecutionContext.md) and its [ServiceContext](../SandboxedExecutionContext.md#getServiceContext)).

`validateStatements` creates a [StatementExecutor](../rest/StatementExecutor.md) to [execute the ParsedStatements](#executeStatements).

In the end, if [failOnNoQueries](#failOnNoQueries) and there was no [QueryContainer](../parser/QueryContainer.md), `validateStatements` throws a `KsqlException`:

```text
The SQL file does not contain any persistent queries.
i.e. it contains no 'INSERT INTO', 'CREATE TABLE x AS SELECT' or
'CREATE STREAM x AS SELECT' style statements.
```

## <span id="executeStatements"> Executing Statements

```java
boolean executeStatements(
  List<ParsedStatement> statements,
  StatementExecutor executor)
```

`executeStatements` requests the given [StatementExecutor](../rest/StatementExecutor.md) to [execute](../rest/StatementExecutor.md#execute) the given `ParsedStatement`s one by one.

In the end, `executeStatements` returns whether there was a `ParsedStatement` with a query.

`executeStatements` is used when:

* `StandaloneExecutor` is requested to [processes a query file](#processesQueryFile) and [validate statements](#validateStatements)
