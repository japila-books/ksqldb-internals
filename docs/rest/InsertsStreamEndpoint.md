# InsertsStreamEndpoint

`InsertsStreamEndpoint` is used to [create an InsertsStreamSubscriber](#createInsertsSubscriber) for [KsqlServerEndpoints](KsqlServerEndpoints.md#createInsertsSubscriber).

## Creating Instance

`InsertsStreamEndpoint` takes the following to be created:

* [KsqlEngine](#ksqlEngine)
* <span id="ksqlConfig"> [KsqlConfig](../KsqlConfig.md)
* <span id="reservedInternalTopics"> [ReservedInternalTopics](ReservedInternalTopics.md)

`InsertsStreamEndpoint` is created when:

* `KsqlServerEndpoints` is requested to [createInsertsSubscriber](KsqlServerEndpoints.md#createInsertsSubscriber)

### <span id="ksqlEngine"> KsqlEngine

`InsertsStreamEndpoint` is given a [KsqlEngine](../KsqlEngine.md) when [created](#creating-instance).

The `KsqlEngine` is used when [creating an InsertsStreamSubscriber](#createInsertsSubscriber) (to [access the MetaStore](../KsqlEngine.md#getMetaStore) to [get a DataSource](#getDataSource)).

## <span id="createInsertsSubscriber"> Creating InsertsStreamSubscriber

```java
InsertsStreamSubscriber createInsertsSubscriber(
  String caseInsensitiveTarget,
  JsonObject properties,
  Subscriber<InsertResult> acksSubscriber,
  Context context,
  WorkerExecutor workerExecutor,
  ServiceContext serviceContext)
```

`createInsertsSubscriber`...FIXME

---

`createInsertsSubscriber` is used when:

* `KsqlServerEndpoints` is requested to [createInsertsSubscriber](KsqlServerEndpoints.md#createInsertsSubscriber)
