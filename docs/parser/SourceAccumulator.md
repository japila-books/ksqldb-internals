# SourceAccumulator

`SourceAccumulator` is a `SqlBaseBaseVisitor` to collect source relations (possibly aliased) in a SQL query.

```text
aliasedRelation
    : relationPrimary (AS? sourceName)?
    ;

relationPrimary
    : sourceName                        #tableName
    ;
```

!!! note
    `SqlBaseBaseVisitor` is generated from `io/confluent/ksql/parser/SqlBase.g4` at build time by [ANTLR](https://www.antlr.org/).

## <span id="visitCreateStream"> visitCreateStream

Creates a [CreateStream](CreateStream.md)
