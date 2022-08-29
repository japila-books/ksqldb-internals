# KafkaTopicClientImpl

`KafkaTopicClientImpl` is a `KafkaTopicClient`.

## Creating Instance

`KafkaTopicClientImpl` takes the following to be created:

* <span id="sharedAdminClient"> `Supplier<Admin>`

`KafkaTopicClientImpl` is created when:

* `KsqlRestApplication` is requested to [buildApplication](rest/KsqlRestApplication.md#buildApplication) (and create a [CommandStore](rest/CommandStore.md#create))
* `PreconditionChecker` is created
* `KsqlRestoreCommandTopic` is created and requested to `maybeCleanUpQuery`
