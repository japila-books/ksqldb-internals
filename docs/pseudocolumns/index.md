# Pseudocolumns

ksqlDB exposes the timestamp, partition and offset of Kafka records as pseudocolumns.

**Pseudocolumns** are [SystemColumns](SystemColumns.md#pseudoColumns) to access record metadata in KSQL statements:

* `ROWTIME`
* `ROWPARTITION`
* `ROWOFFSET`

Pseudocolumns are enabled using [ksql.rowpartition.rowoffset.enabled](../KsqlConfig.md#ksql.rowpartition.rowoffset.enabled)

!!! note "ksqlDB 0.23.1"
    `ROWPARTITION` and `ROWOFFSET` were added in [ksqlDB 0.23.1](https://www.confluent.io/blog/ksqldb-0-23-1-features-updates/#offset-data).
