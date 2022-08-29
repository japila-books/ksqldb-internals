# RouteQuery

`RouteQuery` is an [abstraction](#contract) of [query routers](#implementations).

## Contract

### <span id="routeQuery"> routeQuery

```java
PartitionFetchResult routeQuery(
  KsqlNode node,
  KsqlPartitionLocation location,
  ConfiguredStatement<Query> statement,
  ServiceContext serviceContext,
  RoutingOptions routingOptions,
  Optional<PullQueryExecutorMetrics> pullQueryMetrics,
  PullPhysicalPlan pullPhysicalPlan,
  LogicalSchema outputSchema,
  QueryId queryId,
  PullQueryQueue pullQueryQueue,
  CompletableFuture<Void> shouldCancelRequests,
  Optional<ConsistencyOffsetVector> consistencyOffsetVector)
```

Used when:

* `HARouting` is requested to [executeRounds](HARouting.md#executeRounds)

## Implementations

* [HARouting::executeOrRouteQuery](HARouting.md#executeOrRouteQuery)
