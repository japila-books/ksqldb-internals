# PhysicalPlan

## Creating Instance

`PhysicalPlan` takes the following to be created:

* [Root Node](#root)

`PhysicalPlan` is created when:

* `PhysicalPlanner` is requested to [build a PhysicalPlan](PhysicalPlanner.md#buildPhysicalPlan)

### <span id="root"><span id="getRoot"> Root Node

```java
Node<?> root
```

`PhysicalPlan` is given a root [Node](parser/Node.md) when [created](#creating-instance).

The root node is used when:

* `EngineExecutor` is requested to [plan a Query](EngineExecutor.md#planQuery)
* `ExecutionPlanner` is requested to [build an ExecutionPlan](ExecutionPlanner.md#buildPlan)
