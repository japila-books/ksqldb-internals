# DistributingExecutor

## Creating Instance

`DistributingExecutor` takes the following to be created:

* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="commandQueue"> [CommandQueue](CommandQueue.md)
* <span id="distributedCmdResponseTimeout"> DistributedCmdResponse Timeout
* <span id="injectorFactory"> `BiFunction<KsqlExecutionContext, ServiceContext, Injector>`
* <span id="authorizationValidator"> `KsqlAuthorizationValidator`
* <span id="validatedCommandFactory"> [ValidatedCommandFactory](ValidatedCommandFactory.md)
* <span id="errorHandler"> Error Handler
* <span id="commandRunnerWarning"> CommandRunner Warning

`DistributingExecutor` is created when:

* `KsqlResource` is requested to [configure](KsqlResource.md#configure) (and creates a [RequestHandler](KsqlResource.md#handler))
