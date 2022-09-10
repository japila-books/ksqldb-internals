# Executable

`Executable` is an [abstraction](#contract) of [executables](#implementations).

## Contract

### <span id="awaitTerminated"> awaitTerminated

```java
void awaitTerminated()
```

Used when:

* `KsqlServerMain` is requested to [tryStartApp](KsqlServerMain.md#tryStartApp)
* `MultiExecutable` is requested to `awaitTerminated`

### <span id="notifyTerminated"> notifyTerminated

```java
void notifyTerminated()
```

Used when:

* `KsqlServerMain` is requested to [tryStartApp](KsqlServerMain.md#tryStartApp)
* `MultiExecutable` is requested to `notifyTerminated`

### <span id="shutdown"> shutdown

```java
void shutdown()
```

Used when:

* `KsqlServerMain` is requested to [tryStartApp](KsqlServerMain.md#tryStartApp)
* `MultiExecutable` is requested to `shutdown`

### <span id="startAsync"> startAsync

```java
void startAsync()
```

Used when:

* `KsqlServerMain` is requested to [tryStartApp](KsqlServerMain.md#tryStartApp)
* `MultiExecutable` is requested to `startAsync`

## Implementations

* [StandaloneExecutor](../headless/StandaloneExecutor.md)
* `MultiExecutable`
* `ConnectExecutable`
* [KsqlRestApplication](KsqlRestApplication.md)
