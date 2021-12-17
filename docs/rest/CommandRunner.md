# CommandRunner

## Creating Instance

`CommandRunner` takes the following to be created:

* <span id="statementExecutor"> [InteractiveStatementExecutor](InteractiveStatementExecutor.md)
* <span id="commandStore"> [CommandQueue](CommandQueue.md)
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

* `KsqlRestApplication` utility is used to [build a KsqlRestApplication](KsqlRestApplication.md#buildApplication)

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

For every queued command, `fetchAndRunCommands` [execute the statement](#executeStatement).

### <span id="executeStatement"> Executing Statement

```java
void executeStatement(
  QueuedCommand queuedCommand)
```

`executeStatement` uses the [commandDeserializer](#commandDeserializer) to deserialize a SQL statement (from the `QueuedCommand`).

`executeStatement` prints out the following INFO message to the logs:

```text
Executing statement: [commandStatement]
```

`executeStatement` creates a Java `Runnable` which, when run, requests the [InteractiveStatementExecutor](#statementExecutor) to [handle the statement](InteractiveStatementExecutor.md#handleStatement) and prints out the following INFO message to the logs:

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
