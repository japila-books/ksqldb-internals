# KsqlQueryType

`KsqlQueryType` is a collection (_enum_) of query types in ksqlDB:

* `PERSISTENT`
* [PULL](#PULL)
* `PUSH`

## <span id="PULL"> PULL

`KsqlQueryType.PULL` is used to request the [SlidingWindowRateLimiter](rest/QueryExecutor.md#pullBandRateLimiter) to [allow](SlidingWindowRateLimiter.md#allow) the following:

* [handleStreamPullQuery](rest/QueryExecutor.md#handleStreamPullQuery)
* [handleTablePullQuery](rest/QueryExecutor.md#handleTablePullQuery)
