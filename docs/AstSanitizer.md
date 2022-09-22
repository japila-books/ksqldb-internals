# AstSanitizer

`AstSanitizer` utility is used to [sanitize a Statement](#sanitize).

## <span id="sanitize"> sanitize

```java
Statement sanitize(
  Statement node,
  MetaStore metaStore,
  Boolean lambdaEnabled,
  boolean rowpartitionRowoffsetEnabled)
```

`sanitize` creates a [DataSourceExtractor](DataSourceExtractor.md) to [extractDataSources](DataSourceExtractor.md#extractDataSources) from the given [Statement](parser/Statement.md).

`sanitize` creates a [AstSanitizer.RewriterPlugin](AstSanitizer.RewriterPlugin.md), an [ExpressionRewriterPlugin](ExpressionRewriterPlugin.md) and a `BiFunction` to create a `ExpressionTreeRewriter`.

In the end, `sanitize` creates a `StatementRewriter` to `rewrite` the given `Statement`.

---

`sanitize` is used when:

* `EngineContext` is requested to [prepare a statement](EngineContext.md#prepare)
