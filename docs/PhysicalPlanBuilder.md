# PhysicalPlanBuilder

## Creating Instance

`PhysicalPlanBuilder` takes the following to be created:

* <span id="builder"> `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder))
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="processingLogContext"> `ProcessingLogContext`
* <span id="functionRegistry"> `FunctionRegistry`

`PhysicalPlanBuilder` is created when:

* `QueryEngine` is requested to [build a physical plan](QueryEngine.md#buildPhysicalPlan)

## <span id="buildPhysicalPlan"> Building Physical Plan

```java
PhysicalPlan buildPhysicalPlan(
  LogicalPlanNode logicalPlanNode,
  QueryId queryId,
  Optional<PlanInfo> oldPlanInfo)
```

`buildPhysicalPlan`...FIXME

`buildPhysicalPlan` is used when:

* `QueryEngine` is requested to [build a physical plan](QueryEngine.md#buildPhysicalPlan)
