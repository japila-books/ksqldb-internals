# CommonCreateConfigs

## <span id="FORMAT_PROPERTY"><span id="FORMAT"> FORMAT

## <span id="KAFKA_TOPIC_NAME_PROPERTY"><span id="KAFKA_TOPIC"> KAFKA_TOPIC

The topic that stores the data of the source

Default: (undefined)

## <span id="VALUE_FORMAT_PROPERTY"><span id="VALUE_FORMAT"> VALUE_FORMAT

The format of the serialized value

Default: (undefined)

Overrides [KsqlConfig.KSQL_DEFAULT_VALUE_FORMAT_CONFIG](KsqlConfig.md#KSQL_DEFAULT_VALUE_FORMAT_CONFIG)

Must not be specified with [FORMAT](#FORMAT)

Used when:

* `DefaultSchemaInjector` is requested to `shouldInferSchema`
* `CreateSourceAsProperties` is requested for the [value_format](CreateSourceAsProperties.md#getValueFormat)
* `CreateSourceProperties` is requested for the [value_format](CreateSourceProperties.md#getValueFormat) and to [withFormats](CreateSourceProperties.md#withFormats)

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
