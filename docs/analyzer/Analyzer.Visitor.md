# Analyzer.Visitor

`Analyzer.Visitor` is a [DefaultTraversalVisitor](../parser/DefaultTraversalVisitor.md).

`Analyzer.Visitor` is a `private final class` of [Analyzer](Analyzer.md).

## Creating Instance

`Analyzer.Visitor` takes the following to be created:

* <span id="query"> [Query](../parser/Query.md)
* <span id="persistent"> `persistent` flag

`Analyzer.Visitor` is created when:

* `Analyzer` is requested to [analyze](Analyzer.md#analyze)

## <span id="analyzeNonStdOutSink"> analyzeNonStdOutSink

```java
void analyzeNonStdOutSink(
  Sink sink)
```

`analyzeNonStdOutSink`...FIXME

---

`analyzeNonStdOutSink` is used when:

* `Analyzer` is requested to [analyze a query](Analyzer.md#analyze)
