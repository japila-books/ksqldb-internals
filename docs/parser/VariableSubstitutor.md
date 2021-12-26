# VariableSubstitutor

## <span id="substitute"> Variable Substitution (substitute)

```java
String substitute(
  KsqlParser.ParsedStatement parsedStatement,
  Map<String, String> valueMap)
String substitute(
  String string,
  Map<String, String> valueMap)
```

`substitute` [replaces variables](#replace) in the SQL text (either as a `ParsedStatement` or a `String` using [SqlSubstitutorVisitor](#SqlSubstitutorVisitor)).

`substitute` is used when:

* `Cli` is requested to [substituteVariables](../cli/Cli.md#substituteVariables)
* `EngineContext` is requested to [substituteVariables](../EngineContext.md#substituteVariables)
* `CommandParser` is used

## <span id="SqlSubstitutorVisitor"> SqlSubstitutorVisitor

`SqlSubstitutorVisitor` is a `SqlBaseBaseVisitor` (that `VariableSubstitutor` uses for [variable substitution](#substitute)).

!!! warning "ANTLR"
    `SqlBaseBaseVisitor` is generated from `SqlBase.g4` SQL grammar by ANTLR at build time.

### <span id="replace"> replace

```java
String replace(
  SqlBaseParser.SingleStatementContext singleStatementContext)
```

`replace` walks the statement tree (using `visit`) and then uses `StringSubstitutor.replace` ([Commons Text](https://commons.apache.org/proper/commons-text/javadocs/api-release/org/apache/commons/text/StringSubstitutor.html)) to substitute variables.
