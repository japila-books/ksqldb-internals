# KsqlConfig

## <span id="KSQL_DEFAULT_VALUE_FORMAT_CONFIG"><span id="ksql.persistence.default.format.value"> ksql.persistence.default.format.value

## <span id="KSQL_LAMBDAS_ENABLED"><span id="ksql.lambdas.enabled"> ksql.lambdas.enabled

## <span id="KSQL_OUTPUT_TOPIC_NAME_PREFIX_CONFIG"><span id="ksql.output.topic.name.prefix"> ksql.output.topic.name.prefix

## <span id="KSQL_PULL_QUERIES_ENABLE_CONFIG"><span id="ksql.pull.queries.enable"> ksql.pull.queries.enable

## <span id="KSQL_QUERY_PULL_LIMIT_CLAUSE_ENABLED"><span id="ksql.query.pull.limit.clause.enabled"> ksql.query.pull.limit.clause.enabled

## <span id="KSQL_QUERY_PULL_CONSISTENCY_OFFSET_VECTOR_ENABLED"><span id="ksql.query.pull.consistency.token.enabled"> ksql.query.pull.consistency.token.enabled

## <span id="KSQL_ROWPARTITION_ROWOFFSET_ENABLED"><span id="ksql.rowpartition.rowoffset.enabled"> ksql.rowpartition.rowoffset.enabled

## <span id="KSQL_SERVICE_ID_CONFIG"><span id="ksql.service.id"> ksql.service.id

## <span id="KSQL_SOURCE_TABLE_MATERIALIZATION_ENABLED"><span id="ksql.source.table.materialization.enabled"> ksql.source.table.materialization.enabled

## <span id="KSQL_VARIABLE_SUBSTITUTION_ENABLE"><span id="ksql.variable.substitution.enable"> ksql.variable.substitution.enable

Enables variable substitution in SQL statements

Default: `true`

Used when:

* `Cli` is requested to [isVariableSubstitutionEnabled](cli/Cli.md#isVariableSubstitutionEnabled)
* `RequestHandler` is requested to [isVariableSubstitutionEnabled](rest/RequestHandler.md#isVariableSubstitutionEnabled)
* `RequestValidator` is requested to [isVariableSubstitutionEnabled](rest/RequestValidator.md#isVariableSubstitutionEnabled)
