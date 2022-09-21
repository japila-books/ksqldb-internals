# ProcessingLogConfig

`ProcessingLogConfig` is a `AbstractConfig` ([Apache Kafka]({{ book.kafka }}/AbstractConfig)) with the server properties of the [Processing Log](../processing-log.md) (with [ksql.logging.processing](#propertyName) prefix).

## <span id="TOPIC_AUTO_CREATE"><span id="ksql.logging.processing.topic.auto.create"><span id="topic.auto.create"> topic.auto.create

**ksql.logging.processing.topic.auto.create** enables automatic processing log topic creation.

If `true`, a processing log topic is created when the [ksqlDB server](../rest/KsqlServerMain.md) starts up.

Topic Config | Value
-------------|------
 Name | [ksql.logging.processing.topic.name](#topic.name)
 Number of partitions | [ksql.logging.processing.topic.partitions](#topic.partitions)
 Replication factor | [ksql.logging.processing.topic.replication.factor](#topic.replication.factor)

Default: `false`

Used when:

* `ProcessingLogServerUtils` is requested to [maybeCreateProcessingLogTopic](../rest/ProcessingLogServerUtils.md#maybeCreateProcessingLogTopic)
* `KsqlRestApplication` is requested to [build an application](../rest/KsqlRestApplication.md#buildApplication)
