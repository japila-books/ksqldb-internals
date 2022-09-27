# KsqlConfig

## <span id="KSQL_ENDPOINT_MIGRATE_QUERY_CONFIG"><span id="ksql.endpoint.migrate.query"> ksql.endpoint.migrate.query

Enables [query request migration](rest/StreamedQueryResource.md#shouldMigrateToQueryStream) (from the [/query](api/ServerVerticle.md#uris) endpoint to use the same handler as [/query-stream](api/ServerVerticle.md#uris))

Default: `true`

## <span id="DEFAULT_EXT_DIR"><span id="KSQL_EXT_DIR"><span id="ksql.extension.dir"> ksql.extension.dir

Default: `ext`

## <span id="KSQL_INTERNAL_STREAMS_ERROR_COLLECTOR_CONFIG"><span id="ksql.internal.streams.error.collector"> ksql.internal.streams.error.collector

Used internally to register a new [StreamsErrorCollector](metrics/StreamsErrorCollector.md#create) when `QueryBuilder` is requested to [buildStreamsProperties](QueryBuilder.md#buildStreamsProperties).

The `StreamsErrorCollector` is used for the following:

* [recordError](metrics/StreamsErrorCollector.md#recordError) when [LogMetricAndContinueExceptionHandler](metrics/LogMetricAndContinueExceptionHandler.md) is notified about a deserialization error
* [cleanup](metrics/StreamsErrorCollector.md#cleanup) when `KsqlEngine.CleanupListener` is requested to `onClose`

## <span id="KSQL_LAMBDAS_ENABLED"><span id="ksql.lambdas.enabled"> ksql.lambdas.enabled

Enables lambdas. If `true`, lambdas are processed normally. If `false`, new lambda queries won't be processed but existing lambda queries are unaffected.

Default: `true`

Used when:

* `EngineContext` is requested to [prepare a KSQL statement for execution](EngineContext.md#prepare)

## <span id="KSQL_NEW_QUERY_PLANNER_ENABLED"><span id="ksql.new.query.planner.enabled"> ksql.new.query.planner.enabled

Enables the [new ExecutionPlanner](ExecutionPlanner.md) for [persistent queries](persistent-queries.md)

Default: `false`

[Immutable property](ImmutableProperties.md)

Used when:

* `EngineExecutor` is requested to [throwIfUnsupported](EngineExecutor.md#throwIfUnsupported)

## <span id="KSQL_OUTPUT_TOPIC_NAME_PREFIX_CONFIG"><span id="ksql.output.topic.name.prefix"> ksql.output.topic.name.prefix

## <span id="KSQL_DEFAULT_VALUE_FORMAT_CONFIG"><span id="ksql.persistence.default.format.value"> ksql.persistence.default.format.value

## <span id="KSQL_PULL_QUERIES_ENABLE_CONFIG"><span id="ksql.pull.queries.enable"> ksql.pull.queries.enable

Enables [pull queries](pull-queries.md) on a ksqlDB server

Default: `true`

Used when:

* `ImmutableProperties` is requested for `IMMUTABLE_PROPERTIES`
* `QueryExecutor` is requested to [handle a pull query](rest/QueryExecutor.md#handleQuery)

## <span id="KSQL_QUERY_PULL_CONSISTENCY_OFFSET_VECTOR_ENABLED"><span id="ksql.query.pull.consistency.token.enabled"> ksql.query.pull.consistency.token.enabled

## <span id="KSQL_QUERY_PULL_LIMIT_CLAUSE_ENABLED"><span id="ksql.query.pull.limit.clause.enabled"> ksql.query.pull.limit.clause.enabled

## <span id="KSQL_QUERY_STREAM_PULL_QUERY_ENABLED"><span id="ksql.query.pull.stream.enabled"> ksql.query.pull.stream.enabled

Enables [pull queries on streams](pull-queries.md#stream-pull-queries)

Default: `true`

Used when:

* `KsqlEngine` is requested to [create a stream pull query](KsqlEngine.md#createStreamPullQuery)

## <span id="KSQL_SHARED_RUNTIME_ENABLED"><span id="ksql.runtime.feature.shared.enabled"> ksql.runtime.feature.shared.enabled

Enables shared Kafka Streams runtimes

* `false` - persistent queries will use separate runtimes
* `true` - new queries may share streams instances

Default: `false`

* [ALTER SYSTEM](parser/AlterSystemProperty.md) commands are not supported when turned off ([createForAlterSystemQuery](rest/ValidatedCommandFactory.md#createForAlterSystemQuery))

Used when:

* `EngineExecutor` is requested to [getApplicationId](EngineExecutor.md#getApplicationId)
* `CleanupListener` is requested to `onClose`
* `SandboxedExecutionContext` is requested to [execute a KsqlPlan](SandboxedExecutionContext.md#execute)
* `KafkaStreamsQueryValidator` is requested to `validateCacheBytesUsage`
* `QueryBuilder` is requested to [buildStreamsProperties](QueryBuilder.md#buildStreamsProperties)
* `QueryRegistryImpl` is requested to [createOrReplacePersistentQuery](QueryRegistryImpl.md#createOrReplacePersistentQuery)
* `ValidatedCommandFactory` is requested to [createForPlannedQuery](rest/ValidatedCommandFactory.md#createForPlannedQuery)
* `KsqlResource` is requested to [isValidProperty](rest/KsqlResource.md#isValidProperty)
* `KsqlRestoreCommandTopic` is requested to `maybeCleanUpQuery`
* `KsMaterializationFactory` is requested to `create` a `KsMaterialization`

## <span id="KSQL_ROWPARTITION_ROWOFFSET_ENABLED"><span id="ksql.rowpartition.rowoffset.enabled"> ksql.rowpartition.rowoffset.enabled

Enables `ROWPARTITION` and `ROWOFFSET` pseudo-columns in queries

Default: `true`

Used when:

* `LogicalSchema` is requested to [withPseudoAndKeyColsInValue](LogicalSchema.md#withPseudoAndKeyColsInValue)
* `SystemColumns` is requested to [getPseudoColumnVersionFromConfig](pseudocolumns/SystemColumns.md#getPseudoColumnVersionFromConfig)
* `EngineContext` is requested to [prepare a KSQL statement](EngineContext.md#prepare)
* `EngineExecutor` is requested to [getRowpartitionRowoffsetEnabled](EngineExecutor.md#getRowpartitionRowoffsetEnabled)
* `KsqlEngine` is requested to [getRowpartitionRowoffsetEnabled](KsqlEngine.md#getRowpartitionRowoffsetEnabled)
* `QueryFilterNode` is created
* `QueryProjectNode` is created
* `SchemaKSourceFactory` is requested for a [SchemaKTable](SchemaKSourceFactory.md#buildTable)
* `ScalablePushUtil` is requested to [containsDisallowedColumns](rest/ScalablePushUtil.md#containsDisallowedColumns)

## <span id="SCHEMA_REGISTRY_URL_PROPERTY"><span id="ksql.schema.registry.url"> ksql.schema.registry.url

The URL of the REST endpoint of a schema registry (e.g. [Confluent Schema Registry](https://docs.confluent.io/platform/current/schema-registry/), [Apicurio Schema Registry](https://www.apicur.io/registry/))

Default: (empty)

Used when:

* `SchemaRegisterInjector` is requested to [canRegister](SchemaRegisterInjector.md#canRegister)
* `DefaultSchemaRegistryClient` is requested for `SCHEMA_REGISTRY_CONFIG_NOT_SET`
* `KsqlSchemaRegistryClientFactory` is [created](KsqlSchemaRegistryClientFactory.md#creating-instance)
* `KsqlAvroSerdeFactory` is requested to `getAvroConverter`
* `KsqlJsonSerdeFactory` is requested to `getSchemaConverter`
* `ProtobufSerdeFactory` is requested to `getConverter`
* `SourceBuilderUtils` is requested to `getRegisterCallback`

## <span id="KSQL_SERVICE_ID_CONFIG"><span id="ksql.service.id"> ksql.service.id

The ID of the ksql service.

It is used as prefix for all implicitly named resources created by this instance in Kafka.
By convention, the id should end in a separator character of some form (e.g. a dash or underscore) as this makes identifiers easier to read.

Default: `default_`

Used when:

* `MetricCollectors` is requested to `addConfigurableReporter`
* `DenyListPropertyValidator` is created
* `QueryApplicationId` is requested to [buildInternalTopicPrefix](QueryApplicationId.md#buildInternalTopicPrefix)
* `ReservedInternalTopics` is requested to [processingLogTopic](rest/ReservedInternalTopics.md#processingLogTopic) and [toKsqlInternalTopic](rest/ReservedInternalTopics.md#toKsqlInternalTopic)
* `ServiceInfo` is [created](ServiceInfo.md#create)
* `CleanupListener` is requested to `onClose`
* `OrphanedTransientQueryCleaner` is requested to `cleanupOrphanedInternalTopics`
* `QueryLogger` is requested to [configure](QueryLogger.md#configure)
* `QueryBuilder` is requested to [buildStreamsProperties](QueryBuilder.md#buildStreamsProperties)
* `KsqlRestApplication` is requested to [buildApplication](rest/KsqlRestApplication.md#buildApplication), [setUpHttpMetrics](rest/KsqlRestApplication.md#setUpHttpMetrics)
* `StandaloneExecutorFactory` is [created](headless/StandaloneExecutorFactory.md#create)
* _others_

## <span id="KSQL_SOURCE_TABLE_MATERIALIZATION_ENABLED"><span id="ksql.source.table.materialization.enabled"> ksql.source.table.materialization.enabled

## <span id="KSQL_COLLECT_UDF_METRICS"><span id="ksql.udf.collect.metrics"> ksql.udf.collect.metrics

## <span id="KSQL_UDF_SECURITY_MANAGER_ENABLED"><span id="ksql.udf.enable.security.manager"> ksql.udf.enable.security.manager

## <span id="KSQL_ENABLE_UDFS"><span id="ksql.udfs.enabled"> ksql.udfs.enabled

## <span id="KSQL_VARIABLE_SUBSTITUTION_ENABLE"><span id="ksql.variable.substitution.enable"> ksql.variable.substitution.enable

Enables variable substitution in SQL statements

Default: `true`

Used when:

* `Cli` is requested to [isVariableSubstitutionEnabled](cli/Cli.md#isVariableSubstitutionEnabled)
* `RequestHandler` is requested to [isVariableSubstitutionEnabled](rest/RequestHandler.md#isVariableSubstitutionEnabled)
* `RequestValidator` is requested to [isVariableSubstitutionEnabled](rest/RequestValidator.md#isVariableSubstitutionEnabled)
