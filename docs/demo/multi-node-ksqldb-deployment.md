# Demo: Multi-Node ksqlDB Deployment

This demo shows how to set up a multi-node ksqlDB cluster (with multiple [ksqlDB API server](../rest/KsqlServerMain.md)s).

## Start ksqlDB API server

```console
$ ./bin/ksql-server-start config/ksql-server.properties
...
Server 0.27.2 listening on http://0.0.0.0:8088
```

```console
$ ./bin/ksql http://0.0.0.0:8088
...
CLI v0.27.2, Server v0.27.2 located at http://0.0.0.0:8088
Server Status: RUNNING
...
ksql> server
http://0.0.0.0:8088
```

Leave the ksql session open.

## Start Another Instance of ksqlDB API server

```console
export KSQL_OPTS="-Dlisteners=http://0.0.0.0:8188"
$ ./bin/ksql-server-start config/ksql-server.properties
...
Server 0.27.2 listening on http://0.0.0.0:8188
```

Change the current ksqlDB API server to `http://0.0.0.0:8188`.

```text
ksql> server http://0.0.0.0:8188
Server now: http://0.0.0.0:8188
ksql> server
http://0.0.0.0:8188
```
