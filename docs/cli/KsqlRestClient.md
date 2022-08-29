# KsqlRestClient

`KsqlRestClient` uses Vert.x to talk HTTP 2.0 to the [ksqlDB server](#serverAddress).

## Creating Instance

`KsqlRestClient` takes the following to be created:

* <span id="client"> [KsqlClient](KsqlClient.md)
* <span id="serverAddress"> Address of the ksqlDB server
* <span id="localProps"> Local properties
* <span id="ccloudApiKey"> ccloudApiKey

`KsqlRestClient` is created using [create](#create) utility.

## <span id="create"> create

```java
KsqlRestClient create(
  String serverAddress,
  Map<String, ?> localProps,
  Map<String, String> clientProps,
  Optional<BasicCredentials> creds,
  Optional<BasicCredentials> ccloudApiKey,
  KsqlClientSupplier clientSupplier
)
KsqlRestClient create(
  String serverAddress,
  Map<String, ?> localProps,
  Map<String, String> clientProps,
  Optional<BasicCredentials> creds,
  Optional<BasicCredentials> ccloudApiKey
) // (1)!
```

1. Creates a new [KsqlClient](KsqlClient.md) using `KsqlClientSupplier`

`create` creates a [KsqlClient](KsqlClient.md) (using the `KsqlClientSupplier`) to create a [KsqlRestClient](KsqlRestClient.md).

---

`create` is used when:

* `Ksql` application is [launched](Ksql.md#main)

## <span id="makeKsqlRequest"> makeKsqlRequest

```java
RestResponse<KsqlEntityList> makeKsqlRequest(
  String ksql) // (1)!
RestResponse<KsqlEntityList> makeKsqlRequest(
  String ksql, 
  Long commandSeqNum)
```

1. Uses no `commandSeqNum`

`makeKsqlRequest` [posts the ksql statement](#postKsqlRequest) to the [target](#target).

---

`makeKsqlRequest` is used when:

* `Cli` is requested to [makeKsqlRequest](Cli.md#makeKsqlRequest)
