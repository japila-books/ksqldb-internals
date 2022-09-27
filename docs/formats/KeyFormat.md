# KeyFormat

## Creating Instance

`KeyFormat` takes the following to be created:

* <span id="format"> Format Specification (`FormatInfo`)
* <span id="features"> `SerdeFeatures`
* <span id="window"> Window Specification (`WindowInfo`)

`KeyFormat` is created using the following:

* [nonWindowed](#nonWindowed)
* [windowed](#windowed)
* [of](#of)
* [withSerdeFeatures](#withSerdeFeatures)
* [withoutSerdeFeatures](#withoutSerdeFeatures)

## <span id="of"> Creating KeyFormat Instance

```java
KeyFormat of(
  FormatInfo format,
  SerdeFeatures features,
  Optional<WindowInfo> windowInfo)
```

`of` creates a [KeyFormat](#creating-instance).

---

`of` is used when:

* `DdlCommandExec` is requested for a [KsqlTopic](../DdlCommandExec.md#getKsqlTopic) (for a [CreateSourceCommand](../CreateSourceCommand.md))
* `EngineExecutor` is requested for a [source table plan](../EngineExecutor.md#sourceTablePlan)
* `LogicalPlanner` is requested to [getSinkTopic](../planner/LogicalPlanner.md#getSinkTopic)
* `KsqlAuthorizationValidatorImpl` is requested to `getCreateAsSelectSinkTopic`

## <span id="nonWindowed"> Creating Non-Windowed KeyFormat Instance

```java
KeyFormat nonWindowed(
  FormatInfo format,
  SerdeFeatures features)
```

`nonWindowed` creates a [KeyFormat](#creating-instance) with no [window spec](#window).

---

`nonWindowed` is used when:

* `EngineExecutor` is requested to [getSinkTopic](../EngineExecutor.md#getSinkTopic)
* `JoinNode` is requested to [getDefaultSourceKeyFormat](../planner/JoinNode.md#getDefaultSourceKeyFormat)
* `SerdeFeaturesFactory` is requested to [convertToJsonFormat](SerdeFeaturesFactory.md#convertToJsonFormat)
* `SchemaKStream` is requested to [groupBy](../SchemaKStream.md#groupBy)
* `SchemaKTable` is requested to `groupBy`
* `RuntimeBuildContext` is requested to [buildKeySerde](../RuntimeBuildContext.md#buildKeySerde)

## <span id="withSerdeFeatures"> withSerdeFeatures

```java
KeyFormat withSerdeFeatures(
  SerdeFeatures additionalFeatures)
```

`withSerdeFeatures` copies this `KeyFormat` with the [SerdeFeatures](#features) with the given additional `SerdeFeatures` (possibly overriding the current features).

---

`withSerdeFeatures` is used when:

* `SerdeFeaturesFactory` is requested to [sanitizeKeyFormatWrapping](SerdeFeaturesFactory.md#sanitizeKeyFormatWrapping)
