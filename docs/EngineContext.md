# EngineContext

## <span id="parse"> parse

```java
List<ParsedStatement> parse(
  String sql)
```

`parse` requests the [KsqlParser](#parser) to [parse the SQL text](KsqlParser.md#parse).

`parse` is used when:

* `EngineContext` is requested to [substituteVariables](#substituteVariables)
* `KsqlEngine` is requested to [parse a SQL text](KsqlEngine.md#parse)
* `SandboxedExecutionContext` is requested to `parse`
