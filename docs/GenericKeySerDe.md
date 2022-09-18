# GenericKeySerDe

`GenericKeySerDe` is a [KeySerdeFactory](KeySerdeFactory.md).

## Creating Instance

`GenericKeySerDe` takes the following to be created:

* <span id="innerFactory"> Inner [GenericSerdeFactory](GenericSerdeFactory.md)
* <span id="queryId"> Query ID

`GenericKeySerDe` is created when:

* `CreateSourceFactory` is [created](CreateSourceFactory.md#keySerdeFactory)
* `InsertsSubscriber` is requested to `createInsertsSubscriber`
* `InsertValuesExecutor` is created
* `KafkaConsumerFactory` is requested to create a `KafkaConsumer`
* `MaterializationProviderBuilderFactory` is requested to `buildMaterializationProvider`
* `RuntimeBuildContext` utility is used to [create a RuntimeBuildContext](RuntimeBuildContext.md#of)
