# Injectors

`Injectors` is a collection of [Injector](Injector.md) ordered chains.

## <span id="DEFAULT"> DEFAULT

`DEFAULT` chain is the following:

1. [NO_TOPIC_DELETE](#NO_TOPIC_DELETE)
1. `TopicDeleteInjector`

Used when:

* `KsqlContext` is [created](embedded/KsqlContext.md#create)
* `KsqlResource` is [created](rest/KsqlResource.md#create)

## <span id="NO_TOPIC_DELETE"> NO_TOPIC_DELETE

`NO_TOPIC_DELETE` chain is the following:

1. `DefaultFormatInjector`
1. `SourcePropertyInjector`
1. `DefaultSchemaInjector`
1. `TopicCreateInjector`
1. [SchemaRegisterInjector](SchemaRegisterInjector.md)

Used when:

* `StandaloneExecutorFactory` is [created](rest/StandaloneExecutorFactory.md#create)
