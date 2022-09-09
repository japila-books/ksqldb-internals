# CommonCreateConfigs

## <span id="FORMAT_PROPERTY"><span id="FORMAT"> FORMAT

## <span id="KAFKA_TOPIC_NAME_PROPERTY"><span id="KAFKA_TOPIC"> KAFKA_TOPIC

The topic that stores the data of the source

Default: (undefined)

## <span id="KEY_DELIMITER_PROPERTY"> KEY_DELIMITER_PROPERTY

## <span id="KEY_FORMAT_PROPERTY"><span id="KEY_FORMAT"> KEY_FORMAT

## <span id="KEY_SCHEMA_FULL_NAME"> KEY_SCHEMA_FULL_NAME

## <span id="KEY_SCHEMA_ID"> KEY_SCHEMA_ID

Default: (undefined)

Used when:

* `DefaultSchemaInjector` is requested to `forCreateAsStatement`, `forCreateStatement`, `shouldInferSchema`
* `SchemaRegisterInjector` is requested to [stripSchemaIdConfig](../SchemaRegisterInjector.md#stripSchemaIdConfig), [registerForCreateSource](../SchemaRegisterInjector.md#registerForCreateSource), [registerForCreateAs](../SchemaRegisterInjector.md#registerForCreateAs), [sanityCheck](../SchemaRegisterInjector.md#sanityCheck), [registerRawSchema](../SchemaRegisterInjector.md#registerRawSchema)
* `CreateSourceAsProperties` is requested to [getKeySchemaId](CreateSourceAsProperties.md#getKeySchemaId)
* `CreateSourceProperties` is requested to [getKeySchemaId](CreateSourceProperties.md#getKeySchemaId)

## <span id="SOURCE_NUMBER_OF_REPLICAS"> SOURCE_NUMBER_OF_REPLICAS

## <span id="TIMESTAMP_NAME_PROPERTY"> TIMESTAMP_NAME_PROPERTY

## <span id="TIMESTAMP_FORMAT_PROPERTY"><span id="TIMESTAMP_FORMAT"> TIMESTAMP_FORMAT

## <span id="VALUE_AVRO_SCHEMA_FULL_NAME"> VALUE_AVRO_SCHEMA_FULL_NAME

The fully qualified name of the Avro schema to use for values

Default: (undefined)

## <span id="VALUE_DELIMITER_PROPERTY"> VALUE_DELIMITER_PROPERTY

## <span id="VALUE_FORMAT_PROPERTY"><span id="VALUE_FORMAT"> VALUE_FORMAT

The format of serialized values

Default: (undefined)

Overrides [KsqlConfig.KSQL_DEFAULT_VALUE_FORMAT_CONFIG](../KsqlConfig.md#KSQL_DEFAULT_VALUE_FORMAT_CONFIG)

Must not be specified with [FORMAT](#FORMAT)

Used when:

* `DefaultSchemaInjector` is requested to `shouldInferSchema`
* `CreateSourceAsProperties` is requested for the [VALUE_FORMAT](CreateSourceAsProperties.md#getValueFormat)
* `CreateSourceProperties` is requested for the [VALUE_FORMAT](CreateSourceProperties.md#getValueFormat) and to [withFormats](CreateSourceProperties.md#withFormats)

## <span id="VALUE_SCHEMA_FULL_NAME"> VALUE_SCHEMA_FULL_NAME

The fully-qualified name of the schema of serialized values (e.g. `com.mycorp.mynamespace.sampleRecord`, `io.confluent.ksql.avro_schemas`)

Default: (undefined)

[Must not be specified](#validateKeyValueFormats) together with [VALUE_AVRO_SCHEMA_FULL_NAME](#VALUE_AVRO_SCHEMA_FULL_NAME)

Used when:

* `DefaultSchemaInjector` is requested to [throwOnMultiSchemaDefinitions](../DefaultSchemaInjector.md#throwOnMultiSchemaDefinitions)
* `CreateSourceAsProperties` is requested to [getValueFormatProperties](CreateSourceAsProperties.md#getValueFormatProperties), [withKeyValueSchemaName](CreateSourceAsProperties.md#withKeyValueSchemaName)
* `CreateSourceProperties` is requested to [getValueSchemaFullName](CreateSourceProperties.md#getValueSchemaFullName), [getValueFormatProperties](CreateSourceProperties.md#getValueFormatProperties), [withKeyValueSchemaName](CreateSourceProperties.md#withKeyValueSchemaName)

## <span id="VALUE_SCHEMA_ID"> VALUE_SCHEMA_ID

Default: (undefined)

Used when:

* `CreateSourceAsProperties` is requested for the [VALUE_SCHEMA_ID](CreateSourceAsProperties.md#getValueSchemaId)
* `CreateSourceProperties` is requested for the [VALUE_SCHEMA_ID](CreateSourceProperties.md#getValueSchemaId)
* `DefaultSchemaInjector` is requested to `forCreateAsStatement`, `forCreateStatement`, `shouldInferSchema`
* `SchemaRegisterInjector` is requested to [stripSchemaIdConfig](../SchemaRegisterInjector.md#stripSchemaIdConfig), [registerForCreateSource](../SchemaRegisterInjector.md#registerForCreateSource), [tryGetFormat](../SchemaRegisterInjector.md#tryGetFormat), [registerForCreateAs](../SchemaRegisterInjector.md#registerForCreateAs), [sanityCheck](../SchemaRegisterInjector.md#sanityCheck), [registerRawSchema](../SchemaRegisterInjector.md#registerRawSchema)

## <span id="WRAP_SINGLE_VALUE"> WRAP_SINGLE_VALUE

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
