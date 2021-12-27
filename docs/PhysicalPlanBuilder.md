# PhysicalPlanBuilder

## Creating Instance

`PhysicalPlanBuilder` takes the following to be created:

* <span id="builder"> `StreamsBuilder` ([Kafka Streams]({{ book.kafka_streams }}/kstream/StreamsBuilder))
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)
* <span id="processingLogContext"> `ProcessingLogContext`
* <span id="functionRegistry"> `FunctionRegistry`

`PhysicalPlanBuilder` is created when:

* `QueryEngine` is requested to [buildPhysicalPlan](QueryEngine.md#buildPhysicalPlan)
