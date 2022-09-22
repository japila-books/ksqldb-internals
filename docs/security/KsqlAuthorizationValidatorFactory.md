# KsqlAuthorizationValidatorFactory

`KsqlAuthorizationValidatorFactory` is used by [KsqlRestApplication](../rest/KsqlRestApplication.md) to create the following REST services:

* [WSQueryEndpoint](../rest/WSQueryEndpoint.md#authorizationValidator)
* [StreamedQueryResource](../rest/StreamedQueryResource.md#authorizationValidator)
* [KsqlResource](../rest/KsqlResource.md#authorizationValidator)

## <span id="create"> Creating KsqlAuthorizationValidator

```java
Optional<KsqlAuthorizationValidator> create(
  KsqlConfig ksqlConfig,
  ServiceContext serviceContext,
  Optional<KsqlAuthorizationProvider> externalAuthorizationProvider)
```

`create` [creates a KsqlAccessValidator](#getAccessValidator) that is then used to create a [KsqlAuthorizationValidatorImpl](KsqlAuthorizationValidatorImpl.md) (possibly [cached if enabled](#cacheIfEnabled)).

---

`create` is used when:

* `KsqlRestApplication` is used to [build a KsqlRestApplication](../rest/KsqlRestApplication.md#buildApplication) and then to [startAsync](../rest/KsqlRestApplication.md#startAsync)

### <span id="getAccessValidator"> getAccessValidator

```java
Optional<KsqlAccessValidator> getAccessValidator(
  KsqlConfig ksqlConfig,
  ServiceContext serviceContext,
  Optional<KsqlAuthorizationProvider> externalAuthorizationProvider)
```

`getAccessValidator`...FIXME

## Logging

Enable `ALL` logging level for `io.confluent.ksql.security.KsqlAuthorizationValidatorFactory` logger to see what happens inside.

Add the following line to `config/log4j.properties`:

```text
log4j.logger.io.confluent.ksql.security.KsqlAuthorizationValidatorFactory=ALL
```

Refer to [Logging](../logging.md).
