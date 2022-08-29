# KsqlRestConfig

## <span id="LISTENERS_CONFIG"><span id="listeners"> listeners

## <span id="VERTICLE_INSTANCES"><span id="ksql.verticle.instances"> ksql.verticle.instances

The number of server verticle instances to start per listener.

Default: Twice as much as the number of available CPU cores

Must be at least `1`

Recommended: at least as many instances as there are CPU cores to use, as each instance is single-threaded.

Used when:

* `Server` is requested to [start](../api/Server.md#start)
* `PreconditionServer` is requested to `start`
