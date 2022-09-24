# ExecutionPlanBuilder

## Creating Instance

`ExecutionPlanBuilder` takes the following to be created:

* <span id="builder"> `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/StreamsBuilder))
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="processingLogContext"> [ProcessingLogContext](processing-log/ProcessingLogContext.md) (_not used_)
* <span id="functionRegistry"> [FunctionRegistry](FunctionRegistry.md)

`ExecutionPlanBuilder` is created when:

* `QueryEngine` is requested to [build a physical plan](QueryEngine.md#buildPhysicalPlan)

## <span id="buildPhysicalPlan"> buildPhysicalPlan

```java
ExecutionPlan buildPhysicalPlan(
  LogicalPlanNode logicalPlanNode,
  QueryId queryId,
  Optional<PlanInfo> oldPlanInfo)
```

`buildPhysicalPlan` requests the given `LogicalPlanNode` for an [OutputNode](planner/OutputNode.md) to [build a SchemaKStream](planner/PlanNode.md#buildStream).

??? note "IllegalArgumentException for no OutputNode"
    `buildPhysicalPlan` throws an `IllegalArgumentException` if the `LogicalPlanNode` has got no [OutputNode](planner/OutputNode.md):

    ```text
    Need an output node to build a plan
    ```

In the end, `buildPhysicalPlan` returns an `ExecutionPlan` with the [ExecutionStep](#getSourceStep) of the [SchemaKStream](SchemaKStream.md) and the given `QueryId`.

---

`buildPhysicalPlan` is used when:

* `QueryEngine` is requested to [build a physical plan](QueryEngine.md#buildPhysicalPlan)
