# Ksql

`Ksql` is a [standalone command-line application](#main) (that is executed using [ksql](index.md) shell script).

## <span id="main"> Launching Application

`main` parses command-line options.

In the end, `main` creates a [Ksql](Ksql.md) (with the options, the system properties, a [KsqlRestClient](KsqlRestClient.md#create) and a [Cli](Cli.md#build)) and [runs it](Ksql.md#run).

## Creating Instance

`Ksql` takes the following to be created:

* <span id="options"> `Options`
* <span id="systemProps"> System Properties
* <span id="clientBuilder"> [KsqlClientBuilder](KsqlRestClient.md#create)
* <span id="cliBuilder"> [Cli builder](Cli.md#build)

`Ksql` is created when:

* `Ksql` application is [launched](#main)

## <span id="run"> Executing Statements

```java
int run()
```

`run` loads a config file (if defined).

`run` [builds a KsqlRestClient](#buildClient).

`run` uses the following options to build a [Cli](Cli.md#build) (using the [cliBuilder](#cliBuilder) and the [KsqlRestClient](KsqlRestClient.md)):

* `streamedQueryRowLimit`
* `streamedQueryTimeoutMs`
* `outputFormat` (`JSON` or `TABULAR`)

`run` requests the `Cli` to [add the session variables](Cli.md#addSessionVariables) (based on `definedVars` option).

`run` branches off based on the options:

* With `execute`, `run` requests the `Cli` to [run the command](Cli.md#runCommand)
* With `file`, `run` requests the `Cli` to [run the script file](Cli.md#runScript)
* Otherwise, `run` requests the `Cli` to [run interactively](Cli.md#runInteractively)

### <span id="buildClient"> Building REST Client

```java
KsqlRestClient buildClient(
  Map<String, String> configProps
)
```

`buildClient` uses the following options to [build a KsqlRestClient](KsqlRestClient.md#create) (using the [KsqlClientBuilder](#clientBuilder)):

* `server`
* `user` and `password` for authentication (if needed)
* `confluent-api-key` and `confluent-api-secret` (for Confluent Cloud ksqlDB server if needed)
