# KsqlConfig

## <span id="KSQL_LAMBDAS_ENABLED"><span id="ksql.lambdas.enabled"> ksql.lambdas.enabled

## <span id="KSQL_OUTPUT_TOPIC_NAME_PREFIX_CONFIG"><span id="ksql.output.topic.name.prefix"> ksql.output.topic.name.prefix

## <span id="KSQL_DEFAULT_VALUE_FORMAT_CONFIG"><span id="ksql.persistence.default.format.value"> ksql.persistence.default.format.value

## <span id="KSQL_PULL_QUERIES_ENABLE_CONFIG"><span id="ksql.pull.queries.enable"> ksql.pull.queries.enable

## <span id="KSQL_QUERY_PULL_CONSISTENCY_OFFSET_VECTOR_ENABLED"><span id="ksql.query.pull.consistency.token.enabled"> ksql.query.pull.consistency.token.enabled

## <span id="KSQL_QUERY_PULL_LIMIT_CLAUSE_ENABLED"><span id="ksql.query.pull.limit.clause.enabled"> ksql.query.pull.limit.clause.enabled

## <span id="KSQL_QUERY_STREAM_PULL_QUERY_ENABLED"><span id="ksql.query.pull.stream.enabled"> ksql.query.pull.stream.enabled

Enables [pull queries on streams](pull-queries.md#stream-pull-queries)

Default: `true`

Used when:

* `KsqlEngine` is requested to [create a stream pull query](KsqlEngine.md#createStreamPullQuery)

## <span id="KSQL_ROWPARTITION_ROWOFFSET_ENABLED"><span id="ksql.rowpartition.rowoffset.enabled"> ksql.rowpartition.rowoffset.enabled

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
* `StandaloneExecutorFactory` is [created](rest/StandaloneExecutorFactory.md#create)
* _others_

## <span id="KSQL_SOURCE_TABLE_MATERIALIZATION_ENABLED"><span id="ksql.source.table.materialization.enabled"> ksql.source.table.materialization.enabled

## <span id="KSQL_VARIABLE_SUBSTITUTION_ENABLE"><span id="ksql.variable.substitution.enable"> ksql.variable.substitution.enable

Enables variable substitution in SQL statements

Default: `true`

Used when:

* `Cli` is requested to [isVariableSubstitutionEnabled](cli/Cli.md#isVariableSubstitutionEnabled)
* `RequestHandler` is requested to [isVariableSubstitutionEnabled](rest/RequestHandler.md#isVariableSubstitutionEnabled)
* `RequestValidator` is requested to [isVariableSubstitutionEnabled](rest/RequestValidator.md#isVariableSubstitutionEnabled)
