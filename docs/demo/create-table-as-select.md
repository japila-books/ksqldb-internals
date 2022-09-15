# Demo: CREATE TABLE AS SELECT

This demo shows the internals of [CREATE TABLE AS SELECT](../parser/AstBuilder.Visitor.md#visitCreateTableAs) (_CTAS_).

```text
CREATE (OR REPLACE)? TABLE (IF NOT EXISTS)? sourceName
  (WITH tableProperties)?
  AS query
```

`CREATE TABLE AS SELECT` DDL command is parsed by [AstBuilder](../parser/AstBuilder.Visitor.md#visitCreateTableAs) into [CreateTableAsSelect](../parser/CreateTableAsSelect.md).

`CREATE TABLE AS SELECT` is a [persistent query](../persistent-queries.md).

## Create Stream

=== "KSQL"
    ```sql
    CREATE STREAM product_updates (
      productId VARCHAR,
      price DOUBLE)
    WITH (
      kafka_topic='product_updates',
      value_format='json',
      partitions=1);
    ```

```text
 Message
----------------
 Stream created
----------------
```

## Create Table As Select

=== "KSQL"
    ```sql
    CREATE TABLE product_prices AS
    SELECT
        productId,
        LATEST_BY_OFFSET(price) AS price
    FROM product_updates
    GROUP BY productId;
    ```

```text
 Message
---------------------------------------------
 Created query with ID CTAS_PRODUCT_PRICES_3
---------------------------------------------
```

```text
ksql> LIST QUERIES;

 Query ID              | Query Type | Status    | Sink Name      | Sink Kafka Topic | Query String
-------------------------------------------------------------------------------------------------------------------------------------------------------
 CTAS_PRODUCT_PRICES_3 | PERSISTENT | RUNNING:1 | PRODUCT_PRICES | PRODUCT_PRICES   | CREATE TABLE PRODUCT_PRICES WITH (KAFKA_TOPIC='PRODUCT_PRICES', PARTITIONS=1, REPLICAS=1) AS SELECT   PRODUCT_UPDATES.PRODUCTID PRODUCTID,   LATEST_BY_OFFSET(PRODUCT_UPDATES.PRICE) PRICE FROM PRODUCT_UPDATES PRODUCT_UPDATES GROUP BY PRODUCT_UPDATES.PRODUCTID EMIT CHANGES;
-------------------------------------------------------------------------------------------------------------------------------------------------------
For detailed information on a Query run: EXPLAIN <Query ID>;
```

## List Tables

```text
ksql> list tables;

 Table Name     | Kafka Topic    | Key Format | Value Format | Windowed
------------------------------------------------------------------------
 PRODUCT_PRICES | PRODUCT_PRICES | KAFKA      | JSON         | false
------------------------------------------------------------------------
```

## Send Price Updates

In another terminal, execute the following commands:

```text
echo '1:{"productId": "p1", "price": 20.0}' | kcat -P -b :9092 -t product_updates -K :
echo '2:{"productId": "p2", "price": 10.5}' | kcat -P -b :9092 -t product_updates -K :
```

## List Prices

=== "KSQL"
    ```sql
    SELECT * FROM product_prices;
    ```

```text
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|PRODUCTID                                                                |PRICE                                                                    |
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|p1                                                                       |20.0                                                                     |
|p2                                                                       |10.5                                                                     |
Query terminated
```

## Emit Price Changes

With `EMIT CHANGES` you will receive all price changes per product.

=== "KSQL"
    ```sql
    SELECT * FROM product_prices EMIT CHANGES;
    ```

```text
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|PRODUCTID                                                                |PRICE                                                                    |
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|p1                                                                       |20.0                                                                     |
|p2                                                                       |10.5                                                                     |
```

```text
echo '1:{"productId": "p1", "price": 50.0}' | kcat -P -b :9092 -t product_updates -K :
```

Notice a new entry at the end for the product `p1`.

```text
ksql> SELECT * FROM product_prices EMIT CHANGES;
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|PRODUCTID                                                                |PRICE                                                                    |
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
|p1                                                                       |20.0                                                                     |
|p2                                                                       |10.5                                                                     |
|p1                                                                       |50.0                                                                     |
```

## Explain Persistent Query

=== "KSQL"
    ```sql
    EXPLAIN CTAS_PRODUCT_PRICES_3;
    ```

```text
ksql> EXPLAIN CTAS_PRODUCT_PRICES_3;

ID                   : CTAS_PRODUCT_PRICES_3
Query Type           : PERSISTENT
SQL                  : CREATE TABLE PRODUCT_PRICES WITH (KAFKA_TOPIC='PRODUCT_PRICES', PARTITIONS=1, REPLICAS=1) AS SELECT
  PRODUCT_UPDATES.PRODUCTID PRODUCTID,
  LATEST_BY_OFFSET(PRODUCT_UPDATES.PRICE) PRICE
FROM PRODUCT_UPDATES PRODUCT_UPDATES
GROUP BY PRODUCT_UPDATES.PRODUCTID
EMIT CHANGES;
Host Query Status    : {localhost:8088=RUNNING}

 Field     | Type
------------------------------------
 PRODUCTID | VARCHAR(STRING)  (key)
 PRICE     | DOUBLE
------------------------------------

Sources that this query reads from:
-----------------------------------
PRODUCT_UPDATES

For source description please run: DESCRIBE [EXTENDED] <SourceId>

Sinks that this query writes to:
-----------------------------------
PRODUCT_PRICES

For sink description please run: DESCRIBE [EXTENDED] <SinkId>

Execution plan
--------------
 > [ SINK ] | Schema: PRODUCTID STRING KEY, PRICE DOUBLE | Logger: CTAS_PRODUCT_PRICES_3.PRODUCT_PRICES
   > [ PROJECT ] | Schema: PRODUCTID STRING KEY, PRICE DOUBLE | Logger: CTAS_PRODUCT_PRICES_3.Aggregate.Project
     > [ AGGREGATE ] | Schema: PRODUCTID STRING KEY, PRODUCTID STRING, PRICE DOUBLE, KSQL_AGG_VARIABLE_0 DOUBLE | Logger: CTAS_PRODUCT_PRICES_3.Aggregate.Aggregate
       > [ GROUP_BY ] | Schema: PRODUCTID STRING KEY, PRODUCTID STRING, PRICE DOUBLE | Logger: CTAS_PRODUCT_PRICES_3.Aggregate.GroupBy
         > [ PROJECT ] | Schema: PRODUCTID STRING, PRICE DOUBLE | Logger: CTAS_PRODUCT_PRICES_3.Aggregate.Prepare
           > [ SOURCE ] | Schema: PRODUCTID STRING, PRICE DOUBLE, ROWTIME BIGINT, ROWPARTITION INTEGER, ROWOFFSET BIGINT | Logger: CTAS_PRODUCT_PRICES_3.KsqlTopic.Source


Processing topology
-------------------
Topologies:
   Sub-topology: 0
    Source: KSTREAM-SOURCE-0000000000 (topics: [product_updates])
      --> KSTREAM-TRANSFORMVALUES-0000000001
    Processor: KSTREAM-TRANSFORMVALUES-0000000001 (stores: [])
      --> Aggregate-Prepare
      <-- KSTREAM-SOURCE-0000000000
    Processor: Aggregate-Prepare (stores: [])
      --> KSTREAM-FILTER-0000000003
      <-- KSTREAM-TRANSFORMVALUES-0000000001
    Processor: KSTREAM-FILTER-0000000003 (stores: [])
      --> Aggregate-GroupBy
      <-- Aggregate-Prepare
    Processor: Aggregate-GroupBy (stores: [])
      --> Aggregate-GroupBy-repartition-filter
      <-- KSTREAM-FILTER-0000000003
    Processor: Aggregate-GroupBy-repartition-filter (stores: [])
      --> Aggregate-GroupBy-repartition-sink
      <-- Aggregate-GroupBy
    Sink: Aggregate-GroupBy-repartition-sink (topic: Aggregate-GroupBy-repartition)
      <-- Aggregate-GroupBy-repartition-filter

  Sub-topology: 1
    Source: Aggregate-GroupBy-repartition-source (topics: [Aggregate-GroupBy-repartition])
      --> KSTREAM-AGGREGATE-0000000005
    Processor: KSTREAM-AGGREGATE-0000000005 (stores: [Aggregate-Aggregate-Materialize])
      --> Aggregate-Aggregate-ToOutputSchema
      <-- Aggregate-GroupBy-repartition-source
    Processor: Aggregate-Aggregate-ToOutputSchema (stores: [])
      --> Aggregate-Project
      <-- KSTREAM-AGGREGATE-0000000005
    Processor: Aggregate-Project (stores: [])
      --> KTABLE-TOSTREAM-0000000011
      <-- Aggregate-Aggregate-ToOutputSchema
    Processor: KTABLE-TOSTREAM-0000000011 (stores: [])
      --> KSTREAM-SINK-0000000012
      <-- Aggregate-Project
    Sink: KSTREAM-SINK-0000000012 (topic: PRODUCT_PRICES)
      <-- KTABLE-TOSTREAM-0000000011
```
