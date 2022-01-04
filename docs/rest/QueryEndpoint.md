# QueryEndpoint

## Creating Instance

`QueryEndpoint` takes the following to be created:

* <span id="ksqlEngine"> [KsqlEngine](../KsqlEngine.md)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="pullQueryMetrics"> `PullQueryExecutorMetrics`
* <span id="queryExecutor"> [QueryExecutor](QueryExecutor.md)

`QueryEndpoint` is created when:

* `KsqlServerEndpoints` is requested to [createQueryPublisher](KsqlServerEndpoints.md#createQueryPublisher)
