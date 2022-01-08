# CommandRunner

## Creating Instance

`CommandRunner` takes the following to be created:

* <span id="statementExecutor"> [InteractiveStatementExecutor](InteractiveStatementExecutor.md)
* [CommandQueue](#commandStore)
* <span id="maxRetries"> `maxRetries`
* <span id="clusterTerminator"> `ClusterTerminator`
* <span id="serverState"> `ServerState`
* <span id="ksqlServiceId"> ksql Service ID
* <span id="commandRunnerHealthTimeout"> commandRunnerHealth Timeout
* <span id="metricsGroupPrefix"> metricsGroup Prefix
* [Command Deserializer](#commandDeserializer)
* <span id="errorHandler"> Error Handler
* <span id="kafkaTopicClient"> `KafkaTopicClient`
* <span id="commandTopicName"> Name of the Command Topic
* <span id="metrics"> `Metrics`

`CommandRunner` is created when:

* `KsqlRestApplication` utility is used to [build a KsqlRestApplication](KsqlRestApplication.md#buildApplication-commandRunner)

## <span id="commandStore"> CommandQueue

`CommandRunner` is given a [CommandQueue](CommandQueue.md) when [created](#creating-instance).

The `CommandQueue` is used when `CommandRunner` is requested for the following:

* [processPriorCommands](#processPriorCommands)
* [fetchAndRunCommands](#fetchAndRunCommands)

### <span id="getCommandQueue"> getCommandQueue

```java
CommandQueue getCommandQueue()
```

`getCommandQueue` is used when:

* `KsqlResource` is requested to [configure](KsqlResource.md#configure) and [handleKsqlStatements](KsqlResource.md#handleKsqlStatements)

## <span id="commandDeserializer"> Command Deserializer

`CommandRunner` is given a `Deserializer` (Apache Kafka) of [Command](Command.md)s when [created](#creating-instance).

The `Deserializer` is used in the following:

* [processPriorCommands](#processPriorCommands)
* [start](#start) (to [fetchAndRunCommands](#fetchAndRunCommands) and [executeStatement](#executeStatement))

## <span id="start"> Start Processing Queued Commands

```java
void start()
```

`start` creates and starts a Java thread (on a single-threaded thread pool) to continuously [fetchAndRunCommands](#fetchAndRunCommands).

Every time [fetchAndRunCommands](#fetchAndRunCommands) is executed, the thread prints out the following TRACE message to the logs:

```text
Polling for new writes to command topic
```

`start` is used when:

* `KsqlRestApplication` is requested to [initialize](KsqlRestApplication.md#initialize)

### <span id="fetchAndRunCommands"> Fetching and Running Queued Commands

```java
void fetchAndRunCommands()
```

`fetchAndRunCommands` requests the [CommandQueue](#commandStore) for [new commands](CommandQueue.md#getNewCommands).

`fetchAndRunCommands` prints out the following DEBUG message to the logs:

```text
Found [size] new writes to command topic
```

For every `QueuedCommand`, `fetchAndRunCommands` [executes them](#executeStatement).

### <span id="executeStatement"> Executing Statement

```java
void executeStatement(
  QueuedCommand queuedCommand)
```

`executeStatement` takes the statement from the given `QueuedCommand` and prints out the following INFO message to the logs:

```text
Executing statement: [commandStatement]
```

`executeStatement` creates a `Runnable` ([Java]({{ java.api }}/java/lang/Runnable.html)) which, when run, requests the [InteractiveStatementExecutor](#statementExecutor) to [handle the QueuedCommand](InteractiveStatementExecutor.md#handleStatement) and prints out the following INFO message to the logs:

```text
Executed statement: [commandStatement]
```

## <span id="processPriorCommands"> processPriorCommands

```java
void processPriorCommands(
  PersistentQueryCleanupImpl queryCleanup)
```

`processPriorCommands`...FIXME

`processPriorCommands` is used when:

* `KsqlRestApplication` is requested to [initialize](KsqlRestApplication.md#initialize)
