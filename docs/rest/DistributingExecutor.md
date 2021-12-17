# DistributingExecutor

## Creating Instance

`DistributingExecutor` takes the following to be created:

* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="commandQueue"> [CommandQueue](CommandQueue.md)
* <span id="distributedCmdResponseTimeout"> Distributed Command Response Timeout
* <span id="injectorFactory"> `BiFunction<KsqlExecutionContext, ServiceContext, Injector>`
* <span id="authorizationValidator"> `KsqlAuthorizationValidator`
* <span id="validatedCommandFactory"> [ValidatedCommandFactory](ValidatedCommandFactory.md)
* <span id="errorHandler"> Error Handler
* <span id="commandRunnerWarning"> CommandRunner Warning (`Supplier<String>`)

`DistributingExecutor` is created when:

* `KsqlResource` is requested to [configure](KsqlResource.md#configure) (and creates a [RequestHandler](KsqlResource.md#handler))

## <span id="execute"> Executing Statement

```java
StatementExecutorResponse execute(
  ConfiguredStatement<? extends Statement> statement,
  KsqlExecutionContext executionContext,
  KsqlSecurityContext securityContext)
```

`execute` requests the [injectorFactory](#injectorFactory) to [inject](../Injector.md#inject) into the given `ConfiguredStatement`.

For [InsertInto](../InsertInto.md)s, `execute` [validateInsertIntoQueries](#validateInsertIntoQueries).

`execute` requests the [CommandQueue](#commandQueue) for a [transactional Kafka producer](CommandQueue.md#createTransactionalProducer) (`Producer<CommandId, Command>` to produce to the command topic).

`execute` initialize transactions (using Kafka's [Producer.initTransactions]({{ book.kafka }}/clients/producer/Producer#initTransactions)).

`execute` starts a transaction (using Kafka's [Producer.beginTransaction]({{ book.kafka }}/clients/producer/Producer#beginTransaction)).

`execute` requests the [CommandQueue](#commandQueue) to [waitForCommandConsumer](CommandQueue.md#waitForCommandConsumer).

`execute`...FIXME

`execute` requests the [CommandQueue](#commandQueue) to [enqueue the command](CommandQueue.md#enqueueCommand) and commits the transaction (using Kafka's [Producer.commitTransaction]({{ book.kafka }}/clients/producer/Producer#commitTransaction)).

`execute`...FIXME

`execute` is used when:

* `RequestHandler` is requested to [execute a SQL statement](RequestHandler.md#executeStatement)
