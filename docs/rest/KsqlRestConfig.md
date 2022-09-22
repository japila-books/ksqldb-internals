# KsqlRestConfig

## <span id="DISTRIBUTED_COMMAND_RESPONSE_TIMEOUT_MS_CONFIG"><span id="ksql.server.command.response.timeout.ms"> ksql.server.command.response.timeout.ms

How long to wait for a distributed command to be executed by the local node before returning a response

Default: `5000L`

Used when `KsqlRestApplication` creates the following services (when [building a KsqlRestApplication](#buildApplication) and requested to [startAsync](KsqlRestApplication.md#startAsync)):

* [WSQueryEndpoint](WSQueryEndpoint.md#commandQueueCatchupTimeout)
* [CommandStore](CommandStore.md#commandQueueCatchupTimeout)
* [StreamedQueryResource](StreamedQueryResource.md#commandQueueCatchupTimeout)
* [KsqlResource](KsqlResource.md#distributedCmdResponseTimeout)

## <span id="VERTICLE_INSTANCES"><span id="ksql.verticle.instances"> ksql.verticle.instances

The number of server verticle instances to start per listener.

Default: Twice as much as the number of available CPU cores

Must be at least `1`

Recommended: at least as many instances as there are CPU cores to use, as each instance is single-threaded.

Used when:

* `Server` is requested to [start](../api/Server.md#start)
* `PreconditionServer` is requested to `start`

## <span id="LISTENERS_CONFIG"><span id="listeners"> listeners
