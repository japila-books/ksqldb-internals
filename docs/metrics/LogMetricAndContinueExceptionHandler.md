# LogMetricAndContinueExceptionHandler

`LogMetricAndContinueExceptionHandler` is a `DeserializationExceptionHandler` ([Kafka Streams]({{ book.kafka_streams }}/DeserializationExceptionHandler)).

## <span id="configure"> Configuring

```java
void configure(
  Map<String, ?> configs)
```

`configure` is part of the `DeserializationExceptionHandler` ([Kafka Streams]({{ book.kafka_streams }}/DeserializationExceptionHandler#configure)) abstraction.

---

`configure` finds a [StreamsErrorCollector](#streamsErrorCollector) (using [ksql.internal.streams.error.collector](../KsqlConfig.md#ksql.internal.streams.error.collector) key in the given `configs`).

## <span id="handle"> Handling Deserialization Error

```java
DeserializationHandlerResponse handle(
  ProcessorContext context,
  ConsumerRecord<byte[], byte[]> record,
  Exception exception)
```

`handle` is part of the `DeserializationExceptionHandler` ([Kafka Streams]({{ book.kafka_streams }}/DeserializationExceptionHandler#handle)) abstraction.

---

`handle` prints out the following DEBUG message to the logs:

```text
Exception caught during Deserialization, taskId: [taskId], topic: [topic], partition: [partition], offset: [offset]
```

`handle` requests the [StreamsErrorCollector](#streamsErrorCollector) to [recordError](StreamsErrorCollector.md#recordError) for the topic.

`handle` returns `CONTINUE`.

## Logging

Enable `ALL` logging level for `io.confluent.ksql.errors.LogMetricAndContinueExceptionHandler` logger to see what happens inside.

Add the following line to `conf/log4j.properties`:

```text
log4j.logger.io.confluent.ksql.errors.LogMetricAndContinueExceptionHandler=ALL
```

Refer to [Logging](../logging.md).
