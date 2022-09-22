# KsqlAuthorizationValidatorImpl

`KsqlAuthorizationValidatorImpl` is a [KsqlAuthorizationValidator](KsqlAuthorizationValidator.md) that uses [KsqlAccessValidator](#accessValidator) for [checkTopicAccess](#checkTopicAccess) and [checkSchemaAccess](#checkSchemaAccess).

## Creating Instance

`KsqlAuthorizationValidatorImpl` takes the following to be created:

* [KsqlAccessValidator](#accessValidator)

`KsqlAuthorizationValidatorImpl` is created when:

* `KsqlAuthorizationValidatorFactory` is requested to [create a KsqlAuthorizationValidator](KsqlAuthorizationValidatorFactory.md#create)

### <span id="accessValidator"> KsqlAccessValidator

`KsqlAuthorizationValidatorImpl` is given a [KsqlAccessValidator](KsqlAccessValidator.md) when [created](#creating-instance).

The `KsqlAccessValidator` is used to [checkTopicAccess](#checkTopicAccess) and [checkSchemaAccess](#checkSchemaAccess).

## <span id="checkAuthorization"> checkAuthorization

```java
void checkAuthorization(
  KsqlSecurityContext securityContext,
  MetaStore metaStore,
  Statement statement)
```

`checkAuthorization` is part of the [KsqlAuthorizationValidator](KsqlAuthorizationValidator.md#checkAuthorization) abstraction.

Statement | Handler
----------|--------
 [Query](../parser/Query.md) | [validateQuery](#validateQuery)
 [InsertInto](../parser/InsertInto.md) | [validateInsertInto](#validateInsertInto)
 [CreateAsSelect](../parser/CreateAsSelect.md) | [validateCreateAsSelect](#validateCreateAsSelect)
 [PrintTopic](../parser/PrintTopic.md) | [validatePrintTopic](#validatePrintTopic)
 [CreateSource](../parser/CreateSource.md) | [validateCreateSource](#validateCreateSource)

### <span id="validateInsertInto"> validateInsertInto

```java
void validateInsertInto(
  KsqlSecurityContext securityContext,
  MetaStore metaStore,
  InsertInto insertInto)
```

`validateInsertInto` [validates](#validateQuery) the [query](../parser/InsertInto.md#getQuery) of the given [InsertInto](../parser/InsertInto.md).

`validateInsertInto` [checks WRITE access](#checkTopicAccess) to the [underlying topic](#getSourceTopicName) of the [target](../parser/InsertInto.md#getTarget) of the given [InsertInto](../parser/InsertInto.md).
