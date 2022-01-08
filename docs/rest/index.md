# REST

[KsqlRestApplication](KsqlRestApplication.md) creates a [CommandRunner](KsqlRestApplication.md#commandRunner) to [process prior commands](CommandRunner.md#processPriorCommands) and then [run new commands](CommandRunner.md#fetchAndRunCommands) continuously (off a [CommandQueue](CommandRunner.md#commandStore)).
