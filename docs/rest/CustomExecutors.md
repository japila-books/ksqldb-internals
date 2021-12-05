# CustomExecutors

`CustomExecutors` is a collection of [StatementExecutor](StatementExecutor.md)s that do not need to be distributed.

<span id="CustomExecutors-table">

Enum Name | Class | StatementExecutor
----------|-------|---------
 `DESCRIBE_STREAMS` | `DescribeStreams` | `ListSourceExecutor::describeStreams`
 `DESCRIBE_TABLES` | `DescribeTables` | `ListSourceExecutor::describeTables`
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
 _others_ | |

## <span id="EXECUTOR_MAP"> EXECUTOR_MAP

`CustomExecutors` defines a lookup table of [StatementExecutors by their class types](#CustomExecutors-table).
