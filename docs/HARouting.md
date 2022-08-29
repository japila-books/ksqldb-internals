# HARouting

## Creating Instance

`HARouting` takes the following to be created:

* <span id="routingFilterFactory"> `RoutingFilterFactory`
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="routeQuery"> [RouteQuery](RouteQuery.md) (default: [HARouting::executeOrRouteQuery](#executeOrRouteQuery))

`HARouting` is created when:

* `KsqlRestApplication` is requested to [buildApplication](rest/KsqlRestApplication.md#buildApplication) (and create a [QueryExecutor](rest/QueryExecutor.md#routing))
