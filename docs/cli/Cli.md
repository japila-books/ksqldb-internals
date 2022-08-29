# Cli

## Creating Instance

`Cli` takes the following to be created:

* <span id="streamedQueryRowLimit"> streamedQueryRowLimit (based on `query-row-limit` option)
* <span id="streamedQueryTimeoutMs"> streamedQueryTimeoutMs (based on `query-timeout` option)
* <span id="restClient"> [KsqlRestClient](KsqlRestClient.md)
* <span id="terminal"> `Console`

`Cli` is created using [build](#build) utility.

## <span id="build"> Building Cli Instance

```java
Cli build(
  Long streamedQueryRowLimit,
  Long streamedQueryTimeoutMs,
  OutputFormat outputFormat,
  KsqlRestClient restClient
)
```

`build` builds a `Console` (for the `OutputFormat`) to create a [Cli](#creating-instance).

---

`build` is used when:

* `Ksql` is requested to [run](#run)

## <span id="runCommand"> runCommand

```java
void runCommand(
  String command)
```

`runCommand` [handleLine](#handleLine).

`runCommand` is used when:

* `Ksql` is requested to [run](Ksql.md#run)

## <span id="runInteractively"> runInteractively

```java
void runInteractively()
```

`runInteractively` [displayWelcomeMessage](#displayWelcomeMessage).

`runInteractively` validates the [KsqlRestClient](#restClient).

`runInteractively` [handleLine](#handleLine) until stopped (by a user).

---

`runInteractively` is used when:

* `Ksql` is requested to [run](Ksql.md#run)

## <span id="handleLine"> handleLine

```java
void handleLine(
  String line)
```

`handleLine` removes any leading and trailing spaces from the given `line` and [handleStatements](#handleStatements).

`handleLine` simply returns back when the given `line` is empty after trimming.

---

`handleLine` is used when:

* `Cli` is requested to [runScript](#runScript), [runCommand](#runCommand), [runInteractively](#runInteractively)

### <span id="handleStatements"> handleStatements

```java
void handleStatements(
  String line)
```

`handleStatements` requests the [DefaultKsqlParser](#KSQL_PARSER) to [parse the given line](../parser/KsqlParser.md#parse) (into `ParsedStatement`s).

!!! note
    There could be one or more `ParsedStatement`s in the given `line`.

For every `ParsedStatement`, `handleStatements` [substituteVariables](#substituteVariables) and...FIXME

`handleStatements` validates the statements.

`handleStatements` executes the statements:

1. [substituteVariables](#substituteVariables) (with `isSandbox` flag disabled)
1. Looks up the handler (in the [STATEMENT_HANDLERS](#STATEMENT_HANDLERS)) to handle the statement
    * [makeKsqlRequest](#makeKsqlRequest) (if found) followed by requesting the handler to handle it
    * [makeKsqlRequest](#makeKsqlRequest) only, otherwise

### <span id="substituteVariables"> substituteVariables

```java
ParsedStatement substituteVariables(
  ParsedStatement statement)
```

`substituteVariables`...FIXME

### <span id="isVariableSubstitutionEnabled"> isVariableSubstitutionEnabled

```java
boolean isVariableSubstitutionEnabled()
```

`isVariableSubstitutionEnabled`...FIXME

## <span id="makeKsqlRequest"> makeKsqlRequest

```java
void makeKsqlRequest(
  String statements)
```

`makeKsqlRequest` is part of the `KsqlRequestExecutor` abstraction.

---

`makeKsqlRequest` [makes a ksql request](#makeKsqlRequest-private) with the statements (and the [KsqlRestClient](KsqlRestClient.md#makeKsqlRequest)) and [printKsqlResponse](#printKsqlResponse) to the console.

### <span id="makeKsqlRequest-private"> makeKsqlRequest (private)

```java
RestResponse<R> makeKsqlRequest(
  final String ksql,
  final BiFunction<String, Long, RestResponse<R>> requestIssuer)
```

`makeKsqlRequest` executes the `requestIssuer` binary function (that uses the [KsqlRestClient](KsqlRestClient.md#makeKsqlRequest)) with the given `ksql` (and `commandSequenceNumberToWaitFor` if configured).

`makeKsqlRequest` retires execution of failed statements 10 times.

---

`makeKsqlRequest` is used when:

* `Cli` is requested to [makeKsqlRequest](#makeKsqlRequest), [handleConnectorRequest](#handleConnectorRequest), [handleQuery](#handleQuery), [handlePrintedTopic](#handlePrintedTopic)
