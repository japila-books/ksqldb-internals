# Query

`Query` is a [Statement](Statement.md).

## Creating Instance

`Query` takes the following to be created:

* <span id="location"> `NodeLocation`
* <span id="select"> `Select`
* <span id="from"> `Relation`
* <span id="window"> `WindowExpression`
* <span id="where"> `Expression`
* <span id="groupBy"> `GroupBy`
* <span id="partitionBy"> `PartitionBy`
* <span id="having"> `Expression`
* <span id="refinement"> `RefinementInfo`
* [pullQuery](#pullQuery)
* <span id="limit"> Limit

`Query` is created when:

* `EngineExecutor` is requested to [sourceTablePlan](../EngineExecutor.md#sourceTablePlan)
* `StatementRewriter.Rewriter` is requested to `visitQuery`
* `AstBuilder.Visitor` is requested to [visitQuery](AstBuilder.Visitor.md#visitQuery)

## <span id="pullQuery"> pullQuery Flag

`Query` is given a `pullQuery` flag when [created](#creating-instance) (which is most importantly when `AstBuilder.Visitor` is requested to [visitQuery](AstBuilder.Visitor.md#visitQuery)).

`AstBuilder.Visitor` turns the `pullQuery` flag on (`true`) when the `Query` has no `EMIT` clause (and the [buildingPersistentQuery](AstBuilder.Visitor.md#buildingPersistentQuery) internal flag is off).

### <span id="isPullQuery"> isPullQuery

```java
boolean isPullQuery()
```

`isPullQuery` is used when:

* `Analyzer.Visitor` is requested to [visitQuery](../analyzer/Analyzer.Visitor.md#visitQuery)
* `QueryAnalyzer` is requested to [analyze](../analyzer/QueryAnalyzer.md#analyze)
* `EngineExecutor` is requested to [executeTablePullQuery](../EngineExecutor.md#executeTablePullQuery)
* `StatementRewriter.Rewriter` is requested to `visitQuery`
* `SqlFormatter.Formatter` is requested to `visitQuery`
* `QueryExecutor` is requested to [handleQuery](../rest/QueryExecutor.md#handleQuery)
* `ScalablePushUtil` is requested to [isScalablePushQuery](../rest/ScalablePushUtil.md#isScalablePushQuery)

## <span id="accept"> accept

```java
R accept(
  AstVisitor<R, C> visitor,
  C context)
```

`accept` is part of the [AstNode](AstNode.md#accept) abstraction.

---

`accept` requests the given [AstVisitor](AstVisitor.md) to [visit a Query](AstVisitor.md#visitQuery).

## Demo

### Create KsqlEngine

Create [KsqlEngine](../KsqlEngine.md#demo) with `bootstrap.servers` configuration property (to let the [Injector](../Injector.md) resolve a stream source for a query).

```scala
import io.confluent.ksql.util.KsqlConfig
import scala.jdk.CollectionConverters._
val props = Map("bootstrap.servers" -> ":9092").asJava
val ksqlConfig = new KsqlConfig(props)

val ksqlEngine = ???
```

### Create Stream

```scala
val ksql = """
  CREATE STREAM orders (
    id bigint KEY,
    item varchar)
  WITH (
    kafka_topic = 'orders_topic',
    value_format = 'json',
    partitions = 2);
"""

val statements = ksqlEngine.parse(ksql)
val parsed = statements.asScala.head
val prepared = ksqlEngine.prepare(parsed)
```

```scala
import io.confluent.ksql.statement.Injectors
val serviceContext = ksqlEngine.getServiceContext
val injector = Injectors.DEFAULT(ksqlEngine, serviceContext)
val executionOverrides = Map.empty[String, String].asJava;

import io.confluent.ksql.statement.ConfiguredStatement
import io.confluent.ksql.config.SessionConfig
val preconfigured = ConfiguredStatement.of(
  prepared,
  SessionConfig.of(ksqlConfig, executionOverrides))
val configured = injector.inject(preconfigured)

ksqlEngine.execute(serviceContext, configured)
```

### Pull Query

```scala
val ksql = "SELECT * FROM orders;"

val statements = ksqlEngine.parse(ksql)
val parsed = statements.asScala.head
val prepared = ksqlEngine.prepare(parsed)

val preconfigured = ConfiguredStatement.of(
  prepared,
  SessionConfig.of(ksqlConfig, executionOverrides))
val configured = injector.inject(preconfigured)

import io.confluent.ksql.parser.tree.Query
val query = configured.getStatement.asInstanceOf[Query]
assert(query.isPullQuery)
```
