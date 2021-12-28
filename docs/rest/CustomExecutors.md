# CustomExecutors

`CustomExecutors` is a collection of [StatementExecutor](StatementExecutor.md)s that do not need to be distributed.

`CustomExecutors` is used by [KsqlResource](KsqlResource.md) to [create a RequestHandler](KsqlResource.md#handler) (and [shouldSynchronize](KsqlResource.md#shouldSynchronize) for the `DefaultCommandQueueSync`).

## <span id="EXECUTOR_MAP"> EXECUTOR_MAP

Enum Name | Class | StatementExecutor
----------|-------|---------
                  | `CreateConnector` | `ConnectExecutor::execute`
                  | `DefineVariable` | `VariableExecutor::set`
                  | `DescribeConnector` | `DescribeConnectorExecutor::execute`
                  | `DescribeFunction` | `DescribeFunctionExecutor::execute`
 `DESCRIBE_STREAMS` | `DescribeStreams` | `ListSourceExecutor::describeStreams`
 `DESCRIBE_TABLES` | `DescribeTables` | `ListSourceExecutor::describeTables`
                  | `DropConnector` | `DropConnectorExecutor::execute`
                  | [Explain](#Explain) | `ExplainExecutor::execute`
                  | `InsertValues` | `InsertValuesExecutor::execute`
 `LIST_CONNECTORS` | `ListConnectors` | `ListConnectorsExecutor::execute`
 `LIST_CONNECTOR_PLUGINS` | `ListConnectorPlugins` | `ListConnectorPluginsExecutor::execute`
 `LIST_FUNCTIONS` | `ListFunctions` | `ListFunctionsExecutor::execute`
 `LIST_PROPERTIES` | `ListProperties` | `ListPropertiesExecutor::execute`
 `LIST_QUERIES` | `ListQueries` | `ListQueriesExecutor::execute`
 `LIST_STREAMS` | `ListStreams` | `ListSourceExecutor::streams`
 `LIST_TABLES` | `ListTables` | `ListSourceExecutor::tables`
 `LIST_TOPICS` | `ListTopics` | `ListTopicsExecutor::execute`
 `LIST_TYPES` | `ListTypes` | `ListTypesExecutor::execute`
 `LIST_VARIABLES` | `ListVariables` | `ListVariablesExecutor::execute`
                  | `SetProperty` | `PropertyExecutor::set`
                  | `ShowColumns` | `ListSourceExecutor::columns`
                  | `TerminateQuery` | `TerminateQueryExecutor::execute`
                  | `UndefineVariable` | `VariableExecutor::unset`
                  | `UnsetProperty` | `PropertyExecutor::unset`

### <span id="Explain"> Explain

* [Explain](../parser/Explain.md)
* [ExplainExecutor](ExplainExecutor.md)
