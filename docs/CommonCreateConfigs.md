# CommonCreateConfigs

## <span id="KAFKA_TOPIC_NAME_PROPERTY"><span id="KAFKA_TOPIC"> KAFKA_TOPIC

The topic that stores the data of the source

Default: (undefined)

## <span id="VALUE_FORMAT_PROPERTY"><span id="VALUE_FORMAT"> VALUE_FORMAT

The format of the serialized value

Default: (undefined)

## <span id="SOURCE_NUMBER_OF_PARTITIONS"><span id="PARTITIONS"> PARTITIONS

The number of partitions in the backing topic. Required if creating a source without an existing topic.

Default: (undefined)

## <span id="addToConfigDef"> addToConfigDef

```java
void addToConfigDef(
  ConfigDef configDef,
  boolean topicNameRequired)
```

`addToConfigDef` defines configuration properties (in the given `ConfigDef`).

`addToConfigDef` is used when:

* `CreateAsConfigs` is created
* `CreateConfigs` is created
