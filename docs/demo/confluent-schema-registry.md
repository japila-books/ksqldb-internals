# Demo: Confluent Schema Registry

This demo shows ksqlDB with [Confluent Schema Registry](https://docs.confluent.io/platform/current/schema-registry/) (for Avro schemas).

## Start Confluent Platform

Follow [Quick Start for Confluent Platform](https://docs.confluent.io/platform/current/platform-quickstart.html) to install Confluent Platform that comes with Confluent Schema Registry.

``` console
$ docker compose ps
NAME                COMMAND                  SERVICE             STATUS              PORTS
broker              "/etc/confluent/dock…"   broker              running             0.0.0.0:9092->9092/tcp, 0.0.0.0:9101->9101/tcp
connect             "/etc/confluent/dock…"   connect             running             0.0.0.0:8083->8083/tcp, 9092/tcp
control-center      "/etc/confluent/dock…"   control-center      running             0.0.0.0:9021->9021/tcp
ksql-datagen        "bash -c 'echo Waiti…"   ksql-datagen        running
ksqldb-cli          "/bin/sh"                ksqldb-cli          running
ksqldb-server       "/etc/confluent/dock…"   ksqldb-server       running             0.0.0.0:8088->8088/tcp
rest-proxy          "/etc/confluent/dock…"   rest-proxy          running             0.0.0.0:8082->8082/tcp
schema-registry     "/etc/confluent/dock…"   schema-registry     running             0.0.0.0:8081->8081/tcp
zookeeper           "/etc/confluent/dock…"   zookeeper           running             2888/tcp, 0.0.0.0:2181->2181/tcp, 3888/tcp
```

Schema Registry is included by default with Confluent Platform and should be available at <http://localhost:8081/>.

## Create Schema

Open [Control Center](http://localhost:9021/) and create a new topic (Topics > Add topic) with the name `demo-schema-registry`. Click **Create with defaults** button.

Select **Schema** tab and then click **Set a schema**. Accept the default Avro schema. Click **Create**. That will create **Schema ID: 1** for values.

### List Subjects

=== "HTTPie"

    ```console
    $ http -b http://localhost:8081/subjects/
    [
        "demo-schema-registry-value"
    ]
    ```

=== "curl"

    ```console
    $ curl --silent -X GET http://localhost:8081/subjects/ | jq .
    [
        "demo-schema-registry-value"
    ]
    ```

### List Versions

=== "HTTPie"

    ```console
    http -b http://localhost:8081/subjects/demo-schema-registry-value/versions/latest
    {
        "subject": "demo-schema-registry-value",
        "version": 1,
        "id": 1,
        "schema": "{\"type\":\"record\",\"name\":\"sampleRecord\",\"namespace\":\"com.mycorp.mynamespace\",\"doc\":\"Sample schema to help you get started.\",\"fields\":[{\"name\":\"my_field1\",\"type\":\"int\",\"doc\":\"The int type is a 32-bit signed integer.\"},{\"name\":\"my_field2\",\"type\":\"double\",\"doc\":\"The double type is a double precision (64-bit) IEEE 754 floating-point number.\"},{\"name\":\"my_field3\",\"type\":\"string\",\"doc\":\"The string is a unicode character sequence.\"}]}"
    }
    ```

=== "curl"

    ```console
    $ curl --silent -X GET http://localhost:8081/subjects/demo-schema-registry-value/versions/latest | jq .
    {
        "subject": "demo-schema-registry-value",
        "version": 1,
        "id": 1,
        "schema": "{\"type\":\"record\",\"name\":\"sampleRecord\",\"namespace\":\"com.mycorp.mynamespace\",\"doc\":\"Sample schema to help you get started.\",\"fields\":[{\"name\":\"my_field1\",\"type\":\"int\",\"doc\":\"The int type is a 32-bit signed integer.\"},{\"name\":\"my_field2\",\"type\":\"double\",\"doc\":\"The double type is a double precision (64-bit) IEEE 754 floating-point number.\"},{\"name\":\"my_field3\",\"type\":\"string\",\"doc\":\"The string is a unicode character sequence.\"}]}"
        }
    ```

### Query by Schema ID

=== "HTTPie"

    ```console
    $ http -b http://localhost:8081/schemas/ids/1 | jq .schema
    "{\"type\":\"record\",\"name\":\"sampleRecord\",\"namespace\":\"com.mycorp.mynamespace\",\"doc\":\"Sample schema to help you get started.\",\"fields\":[{\"name\":\"my_field1\",\"type\":\"int\",\"doc\":\"The int type is a 32-bit signed integer.\"},{\"name\":\"my_field2\",\"type\":\"double\",\"doc\":\"The double type is a double precision (64-bit) IEEE 754 floating-point number.\"},{\"name\":\"my_field3\",\"type\":\"string\",\"doc\":\"The string is a unicode character sequence.\"}]}"
    ```

=== "curl"

    ```console
    $ curl --silent -X GET http://localhost:8081/schemas/ids/1 | jq .
    {
        "schema": "{\"type\":\"record\",\"name\":\"sampleRecord\",\"namespace\":\"com.mycorp.mynamespace\",\"doc\":\"Sample schema to help you get started.\",\"fields\":[{\"name\":\"my_field1\",\"type\":\"int\",\"doc\":\"The int type is a 32-bit signed integer.\"},{\"name\":\"my_field2\",\"type\":\"double\",\"doc\":\"The double type is a double precision (64-bit) IEEE 754 floating-point number.\"},{\"name\":\"my_field3\",\"type\":\"string\",\"doc\":\"The string is a unicode character sequence.\"}]}"
    }
    ```

## Create Table

```console
$ docker compose exec ksqldb-cli \
    ksql http://ksqldb-server:8088 \
      --execute 'LIST PROPERTIES' | grep ksql.schema.registry.url
 ksql.schema.registry.url                                   | KSQL  | SERVER           | http://schema-registry:8081
```

```console
docker compose exec ksqldb-cli ksql http://ksqldb-server:8088
```

```sql
CREATE TABLE demo_schema_registry (
    rowkey varchar PRIMARY KEY)
WITH (
    KAFKA_TOPIC = 'demo-schema-registry',
    VALUE_FORMAT = 'avro');
```

You should see the following message:

```text
 Message
---------------
 Table created
---------------
```

If a topic or a schema are not available, you may face the following error message:

```text
Schema for message values on topic 'demo-schema-registry' does not exist in the Schema Registry.
Subject: demo-schema-registry-value
Possible causes include:
- The topic itself does not exist
	-> Use SHOW TOPICS; to check
- Messages on the topic are not serialized using a format Schema Registry supports
	-> Use PRINT 'demo-schema-registry' FROM BEGINNING; to verify
- Messages on the topic have not been serialized using a Confluent Schema Registry supported serializer
	-> See https://docs.confluent.io/current/schema-registry/docs/serializer-formatter.html
- The schema is registered on a different instance of the Schema Registry
	-> Use the REST API to list available subjects	https://docs.confluent.io/current/schema-registry/docs/api.html#get--subjects
- You do not have permissions to access the Schema Registry.
	-> See https://docs.confluent.io/current/schema-registry/docs/security.html
```

## Insert Values

Insert data into the table using the following `INSERT INTO VALUES` command.

```sql
INSERT INTO demo_schema_registry (rowkey, my_field1, my_field2, my_field3)
VALUES ('0', 1, 2.3, 'demo');
```

### NAME_MISMATCH

It will likely fail because the `name` and the `namespace` of the schema (that you created in Schema Registry) do not match.

```text
Failed to insert values into 'DEMO_SCHEMA_REGISTRY'. Could not serialize value: [ 1 | 2.3 | 'demo' ]. Error serializing message to topic: demo-schema-registry. Failed to access Avro data from topic demo-schema-registry : Schema being registered is incompatible with an earlier schema for subject "demo-schema-registry-value", details: [Incompatibility{type:NAME_MISMATCH, location:/name, message:expected: com.mycorp.mynamespace.sampleRecord, reader:{"type":"record","name":"KsqlDataSourceSchema","namespace":"io.confluent.ksql.avro_schemas","fields":[{"name":"MY_FIELD1","type":["null","int"],"default":null},{"name":"MY_FIELD2","type":["null","double"],"default":null},{"name":"MY_FIELD3","type":["null","string"],"default":null}]}, writer:{"type":"record","name":"sampleRecord","namespace":"com.mycorp.mynamespace","doc":"Sample schema to help you get started.","fields":[{"name":"my_field1","type":"int","doc":"The int type is a 32-bit signed integer."},{"name":"my_field2","type":"double","doc":"The double type is a double precision (64-bit) IEEE 754 floating-point number."},{"name":"my_field3","type":"string","doc":"The string is a unicode character sequence."}]}}]; error code: 409
```

### Recreate Avro Schema

Re-create the Avro schema in Schema Registry with the following and `INSERT INTO VALUES` should work fine now.

Property | Value
---------|------
 name    | KsqlDataSourceSchema
 namespace | io.confluent.ksql.avro_schemas

### (Optional) Recreate Table with VALUE_AVRO_SCHEMA_FULL_NAME

You can use [VALUE_AVRO_SCHEMA_FULL_NAME](../parser/CommonCreateConfigs.md#VALUE_AVRO_SCHEMA_FULL_NAME) or [VALUE_SCHEMA_FULL_NAME](../parser/CommonCreateConfigs.md#VALUE_SCHEMA_FULL_NAME) properties when `CREATE TABLE`.

```sql
DROP TABLE demo_schema_registry;
```

```sql
CREATE TABLE demo_schema_registry (
    rowkey varchar PRIMARY KEY)
WITH (
    KAFKA_TOPIC = 'demo-schema-registry',
    VALUE_FORMAT = 'avro',
    VALUE_AVRO_SCHEMA_FULL_NAME = 'com.mycorp.mynamespace.sampleRecord');
```

```sql
INSERT INTO demo_schema_registry (rowkey, my_field1, my_field2, my_field3)
VALUES ('0', 1, 2.3, 'demo');
```

## Print Records

```text
ksql> PRINT `demo-schema-registry` FROM BEGINNING LIMIT 1;
Key format: JSON or KAFKA_STRING
Value format: AVRO or KAFKA_STRING
rowtime: 2022/09/06 12:19:42.473 Z, key: 0, value: {"MY_FIELD1": 1, "MY_FIELD2": 2.3, "MY_FIELD3": "demo"}, partition: 0
Topic printing ceased
```
