# Cli

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

`runInteractively`...FIXME

`runInteractively` is used when:

* `Ksql` is requested to [run](Ksql.md#run)

## <span id="handleLine"> handleLine

```java
void handleLine(
  String line)
```

`handleLine` removes any leading and trailing spaces from the given `line` and [handleStatements](#handleStatements).

`handleLine` simply returns back when the given `line` is empty after trimming.

`handleLine` is used when:

* `Cli` is requested to [runScript](#runScript), [runCommand](#runCommand), [runInteractively](#runInteractively)

### <span id="handleStatements"> handleStatements

```java
void handleStatements(
  String line)
```

`handleStatements` requests the [DefaultKsqlParser](#KSQL_PARSER) to [parse the given line](../KsqlParser.md#parse) (into `ParsedStatement`s).

!!! note
    There could be one or more `ParsedStatement`s in the given `line`.

For every `ParsedStatement`, `handleStatements` [substituteVariables](#substituteVariables) and...FIXME

`handleStatements`...FIXME

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
