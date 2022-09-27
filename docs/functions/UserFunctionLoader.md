# UserFunctionLoader

`UserFunctionLoader` uses [Function Loaders](#function-loaders) to [loadFunctions](#loadFunctions).

## Creating Instance

`UserFunctionLoader` takes the following to be created:

* <span id="functionRegistry"> `MutableFunctionRegistry`
* <span id="pluginDir"> Plugin Directory
* <span id="parentClassLoader"> Parent `ClassLoader`
* <span id="blacklist"> blacklist
* <span id="metrics"> `Metrics`
* <span id="loadCustomerUdfs"> `loadCustomerUdfs` flag

`UserFunctionLoader` is created when:

* `UdfLoaderUtil` is requested to [load](UdfLoaderUtil.md#load)
* `UserFunctionLoader` is requested to [newInstance](#newInstance)

### <span id="udafLoader"><span id="udfLoader"><span id="udtfLoader"><span id="function-loaders"> Function Loaders

`UserFunctionLoader` creates the function loaders when [created](#creating-instance):

* [UdafLoader](UdafLoader.md)
* [UdfLoader](UdfLoader.md)
* [UdtfLoader](UdtfLoader.md)

The loaders are used to [loadFunctions](#loadFunctions).

## <span id="newInstance"> Creating UserFunctionLoader

```java
UserFunctionLoader newInstance(
  KsqlConfig config,
  MutableFunctionRegistry metaStore,
  String ksqlInstallDir,
  Metrics metricsRegistry)
```

With [ksql.udf.enable.security.manager](../KsqlConfig.md#KSQL_UDF_SECURITY_MANAGER_ENABLED) enabled, `newInstance` sets the System security as `ExtensionSecurityManager`.

In the end, `newInstance` creates a [UserFunctionLoader](#creating-instance) with the following:

* The given `MutableFunctionRegistry`
* The plugin directory as [ext](../KsqlConfig.md#DEFAULT_EXT_DIR) under the given `ksqlInstallDir` unless [ksql.extension.dir](../KsqlConfig.md#KSQL_EXT_DIR) is defined
* A new `Blacklist` with `resource-blacklist.txt` under the plugin directory
* The given `Metrics` if [ksql.udf.collect.metrics](../KsqlConfig.md#KSQL_COLLECT_UDF_METRICS)
* [ksql.udfs.enabled](../KsqlConfig.md#KSQL_ENABLE_UDFS)

---

`newInstance` is used when:

* `KsqlContext` is used to [create a KsqlContext](../embedded/KsqlContext.md#create)
* `KsqlRestApplication` is used to [build a KsqlRestApplication](../rest/KsqlRestApplication.md#create)
* `StandaloneExecutorFactory` is used to [create a StandaloneExecutor](../headless/StandaloneExecutorFactory.md#create)

## <span id="load"> Loading Function Jars

```java
void load()
```

`load` [loadFunctions](#loadFunctions).

With [loadCustomerUdfs](#loadCustomerUdfs) enabled, `load` finds all the jar files in the [pluginDir](#pluginDir) and [loadFunctions](#loadFunctions) for every jar file (unless [blacklisted](#blacklist)).

---

`load` is used when:

* `KsqlContext` is used to [create a KsqlContext](../embedded/KsqlContext.md#create)
* `UdfLoaderUtil` is requested to [load](UdfLoaderUtil.md#load)
* `KsqlRestApplication` is used to [build a KsqlRestApplication](../rest/KsqlRestApplication.md#create)
* `StandaloneExecutor` is requested to [startAsync](../headless/StandaloneExecutor.md#startAsync)

### <span id="loadFunctions"> loadFunctions

```java
void loadFunctions(
  ClassLoader loader,
  Optional<Path> path)
```

!!! note "ClassGraph"
    `loadFunctions` uses [ClassGraph](https://github.com/classgraph/classgraph) library to scan the classpath.

`loadFunctions` finds classes with the function annotations and uses the [UdfLoader](#udfLoader) to load them.

Annotation | Loader
-----------|-------
 [UdfDescription](UdfDescription.md) | [UdfLoader](UdfLoader.md#loadUdfFromClass)
 [UdafDescription](UdafDescription.md) | [UdafLoader](UdafLoader.md#loadUdafFromClass)
 [UdtfDescription](UdtfDescription.md) | [UdtfLoader](UdtfLoader.md#loadUdtfFromClass)

## Logging

Enable `ALL` logging level for `io.confluent.ksql.function.UserFunctionLoader` logger to see what happens inside.

Add the following line to `config/log4j.properties`:

```text
log4j.logger.io.confluent.ksql.function.UserFunctionLoader=ALL
```

Refer to [Logging](../logging.md).
