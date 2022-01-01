# SchemaKStream

## Creating Instance

`SchemaKStream` takes the following to be created:

* <span id="sourceStep"> [ExecutionStep](ExecutionStep.md) of `KStreamHolder<K>`
* <span id="schema"> `LogicalSchema`
* <span id="keyFormat"> `KeyFormat`
* <span id="ksqlConfig"> [KsqlConfig](KsqlConfig.md)
* <span id="functionRegistry"> `FunctionRegistry`

`SchemaKStream` is created when:

* `SchemaKSourceFactory` is requested to [schemaKStream](SchemaKSourceFactory.md#schemaKStream)
* `SchemaKStream` is requested to [into](#into), [filter](#filter), [flatMap](#flatMap) and _others_
