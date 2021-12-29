# Query Planning

ksqlDB uses [EngineExecutor](../EngineExecutor.md) to [plan a query for execution](../EngineExecutor.md#planQuery).

At some point in a query lifecycle, [QueryEngine](../QueryEngine.md) is requested for a [physical query plan](../QueryEngine.md#buildPhysicalPlan) (that uses Kafka Streams' [StreamsBuilder]({{ book.kafka_streams }}/kstream/StreamsBuilder) to build a streaming topology).
