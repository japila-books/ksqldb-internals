# KsqlAccessValidator

`KsqlAccessValidator` is an [abstraction](#contract) of [access validators](#implementations) to check [subject](#checkSubjectAccess) and [topic](#checkTopicAccess) access.

## Contract

### <span id="checkSubjectAccess"> checkSubjectAccess

```java
void checkSubjectAccess(
  KsqlSecurityContext securityContext,
  String subjectName,
  AclOperation operation)
```

Used when:

* `KsqlAuthorizationValidatorImpl` is requested to [checkSchemaAccess](KsqlAuthorizationValidatorImpl.md#checkSchemaAccess)
* `KsqlCacheAccessValidator` is requested to `internalSubjectAccessValidator`

### <span id="checkTopicAccess"> checkTopicAccess

```java
void checkTopicAccess(
  KsqlSecurityContext securityContext,
  String topicName,
  AclOperation operation)
```

Used when:

* `KsqlAuthorizationValidatorImpl` is requested to [checkTopicAccess](KsqlAuthorizationValidatorImpl.md#checkTopicAccess)
* `KsqlCacheAccessValidator` is requested to `internalTopicAccessValidator`

## Implementations

* `KsqlBackendAccessValidator`
* `KsqlCacheAccessValidator`
* `KsqlProvidedAccessValidator`
