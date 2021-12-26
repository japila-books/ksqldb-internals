# Query Parsing

ksqlDB uses [AstBuilder](AstBuilder.md) to parse and [build Statement tree nodes](AstBuilder.md#buildStatement) from SQL text (possibly with multiple SQL statements separated by `;`).

ksqlDB uses [DefaultKsqlParser](DefaultKsqlParser.md) to [prepare a statement for execution](DefaultKsqlParser.md#prepare).

`EngineContext` is used for [Variable Substitution](../EngineContext.md#substituteVariables) and [sanitize](../AstSanitizer.md#sanitize) while [preparing a statement for execution](../EngineContext.md#prepare).
