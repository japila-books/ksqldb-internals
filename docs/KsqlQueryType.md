# KsqlQueryType

`KsqlQueryType` is a collection (_enum_) of query types in ksqlDB:

* `PERSISTENT`
* [PULL](#PULL)
* [PUSH](#PUSH)

## <span id="PULL"> PULL

`KsqlQueryType.PULL` is used to request the [SlidingWindowRateLimiter](rest/QueryExecutor.md#pullBandRateLimiter) to [allow](SlidingWindowRateLimiter.md#allow) the following:

* [handleStreamPullQuery](rest/QueryExecutor.md#handleStreamPullQuery)
* [handleTablePullQuery](rest/QueryExecutor.md#handleTablePullQuery)

## <span id="PUSH"> PUSH

`KsqlQueryType.PULL` is used to request the [SlidingWindowRateLimiter](rest/QueryExecutor.md#pullBandRateLimiter) to [allow](SlidingWindowRateLimiter.md#allow) the following:

* [handleScalablePushQuery](rest/QueryExecutor.md#handleScalablePushQuery)

`KsqlQueryType.PUSH` is the [query type](TransientQueryMetadata.md#getQueryType) of [TransientQueryMetadata](TransientQueryMetadata.md).
