# RecordFormatter

## Creating Instance

`RecordFormatter` takes the following to be created:

* <span id="schemaRegistryClient"> `SchemaRegistryClient`
* <span id="topicName"> Topic Name

`RecordFormatter` is created when:

* `PrintPublisher` is requested to `subscribe` (to create a `PrintSubscription`)
* `TopicStreamWriter` is requested to `write` (to create a `PrintSubscription`)

## <span id="format"> format

```java
List<String> format(
  Iterable<ConsumerRecord<Bytes, Bytes>> records)
```

`format`...FIXME

---

`format` is used when:

* `PrintSubscription` is requested to `poll`
* `TopicStreamWriter` is requested to `write`
