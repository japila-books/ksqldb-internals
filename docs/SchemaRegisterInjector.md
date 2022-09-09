# SchemaRegisterInjector

`SchemaRegisterInjector` is an [Injector](Injector.md).

`SchemaRegisterInjector` handles the following [Statement](parser/Statement.md)s:

* [CreateAsSelect](parser/CreateAsSelect.md)
* [CreateSource](parser/CreateSource.md)

`SchemaRegisterInjector` relies on the following [overrides](SessionConfig.md#getOverrides) (of the [SessionConfig](SessionConfig.md) of the given `ConfiguredStatement`):

* [KEY_SCHEMA_ID](parser/CommonCreateConfigs.md#KEY_SCHEMA_ID)
* [VALUE_SCHEMA_ID](parser/CommonCreateConfigs.md#VALUE_SCHEMA_ID)

## Creating Instance

`SchemaRegisterInjector` takes the following to be created:

* <span id="executionContext"> [KsqlExecutionContext](KsqlExecutionContext.md)
* <span id="serviceContext"> [ServiceContext](ServiceContext.md)

`SchemaRegisterInjector` is created when:

* `Injectors` is requested for [NO_TOPIC_DELETE chain](Injectors.md#NO_TOPIC_DELETE)

## <span id="inject"> Injecting Metadata

```java
<T extends Statement> ConfiguredStatement<T> inject(
  ConfiguredStatement<T> statement)
```

`inject` is part of the [Injector](Injector.md#inject) abstraction.

---

`inject` branches off based on the type of the given [Statement](parser/Statement.md):

* [registerForCreateAs](#registerForCreateAs) for [CreateAsSelect](parser/CreateAsSelect.md)
* [registerForCreateSource](#registerForCreateSource) for [CreateSource](parser/CreateSource.md)

In the end, `inject` [stripSchemaIdConfig](#stripSchemaIdConfig).

## <span id="registerForCreateAs"> registerForCreateAs

```java
void registerForCreateAs(
  ConfiguredStatement<? extends CreateAsSelect> cas)
```

`registerForCreateAs`...FIXME

---

`registerForCreateAs` is used when:

* `SchemaRegisterInjector` is requested to [inject](#inject) (with a [CreateAsSelect](parser/CreateAsSelect.md) statement)

## <span id="registerForCreateSource"> registerForCreateSource

```java
void registerForCreateSource(
  ConfiguredStatement<? extends CreateSource> cs)
```

`registerForCreateSource` takes the [CreateSource](parser/CreateSource.md) (from the given `ConfiguredStatement`) and builds a `LogicalSchema` (based on `TableElement`s).

`registerForCreateSource` builds `FormatInfo`s for the [keys](parser/SourcePropertiesUtil.md#getKeyFormat) and [values](parser/SourcePropertiesUtil.md#getValueFormat) and [converts them to Format](#tryGetFormat).

`registerForCreateSource` builds `SerdeFeatures` for the values.

`registerForCreateSource` takes the `rawKeySchema` and the `rawValueSchema` from [overrides](SessionConfig.md#getOverrides) (of the [SessionConfig](SessionConfig.md) of the given `ConfiguredStatement` if available).

In the end, `registerForCreateSource` [registerSchemas](#registerSchemas) (unless already registered).

---

`registerForCreateSource` is used when:

* `SchemaRegisterInjector` is requested to [inject](#inject) (with a [CreateSource](parser/CreateSource.md) statement)

### <span id="tryGetFormat"> tryGetFormat

```java
Format tryGetFormat(
  FormatInfo formatInfo,
  boolean isKey,
  String statementText)
```

`tryGetFormat`...FIXME

## <span id="stripSchemaIdConfig"> stripSchemaIdConfig

```java
<T extends Statement> ConfiguredStatement<T> stripSchemaIdConfig(
  ConfiguredStatement<T> statement)
```

`stripSchemaIdConfig` requests the [SessionConfig](SessionConfig.md) (of the given `ConfiguredStatement`) for [overrides](SessionConfig.md#getOverrides).

`stripSchemaIdConfig` removes [KEY_SCHEMA_ID](parser/CommonCreateConfigs.md#KEY_SCHEMA_ID) and [VALUE_SCHEMA_ID](parser/CommonCreateConfigs.md#VALUE_SCHEMA_ID) from the overrides (and creates a new `ConfiguredStatement`) if specified. Otherwise, with no [KEY_SCHEMA_ID](parser/CommonCreateConfigs.md#KEY_SCHEMA_ID) and no [VALUE_SCHEMA_ID](parser/CommonCreateConfigs.md#VALUE_SCHEMA_ID), `stripSchemaIdConfig` does nothing and simply returns the given `ConfiguredStatement`.

---

`stripSchemaIdConfig` is used when:

* `SchemaRegisterInjector` is requested to [inject](#inject)

## <span id="registerSchemas"> registerSchemas

```java
void registerSchemas(
  LogicalSchema schema,
  Pair<SchemaAndId, SchemaAndId> kvRawSchema,
  String kafkaTopic,
  FormatInfo keyFormat,
  SerdeFeatures keySerdeFeatures,
  FormatInfo valueFormat,
  SerdeFeatures valueSerdeFeatures,
  KsqlConfig config,
  String statementText,
  boolean registerIfSchemaExists)
```

`registerSchemas`...FIXME

---

`registerSchemas` is used when:

* `SchemaRegisterInjector` is requested to [registerForCreateSource](#registerForCreateSource) and [registerForCreateAs](#registerForCreateAs)

### <span id="registerSchema"> registerSchema

```java
void registerSchema(
  List<? extends SimpleColumn> schema,
  String topic,
  FormatInfo formatInfo,
  SerdeFeatures serdeFeatures,
  KsqlConfig config,
  String statementText,
  boolean registerIfSchemaExists,
  String subject,
  boolean isKey)
```

`registerSchema`...FIXME

### <span id="registerRawSchema"> registerRawSchema

```java
void registerRawSchema(
  SchemaAndId schemaAndId,
  String topic,
  String statementText,
  String subject,
  Boolean isKey)
```

`registerRawSchema`...FIXME

### <span id="sanityCheck"> sanityCheck

```java
void sanityCheck(
  SchemaAndId schemaAndId,
  FormatInfo formatInfo,
  String topic,
  KsqlConfig config,
  String statementText,
  boolean isKey)
```

`sanityCheck`...FIXME

## <span id="canRegister"> canRegister

```java
boolean canRegister(
  Format format,
  KsqlConfig config,
  String topic)
```

`canRegister` is `true` unless the given [Format](Format.md) does not [support](Format.md#supportsFeature) `SCHEMA_INFERENCE` feature.

`canRegister` throws a `KsqlSchemaRegistryNotConfiguredException` unless [ksql.schema.registry.url](KsqlConfig.md#SCHEMA_REGISTRY_URL_PROPERTY) is defined (non-empty):

```text
Cannot create topic '[topic]' with format [name] without configuring 'ksql.schema.registry.url'
```

---

`canRegister` is used when:

* `SchemaRegisterInjector` is requested to [sanityCheck](#sanityCheck) and [registerSchema](#registerSchema)
