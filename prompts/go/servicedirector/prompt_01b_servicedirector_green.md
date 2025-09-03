# **Prompt 01: Implementation for ServiceDirector**

**Objective:** Generate the implementation for the `ServiceDirector` microservice. The generated code must satisfy the previously defined integration test.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicedirector/sdapp.go` and `servicedirector/sdconfig.go` files.

Your implementation of the `ServiceDirector` must make the integration test pass by correctly listening for Pub/Sub commands, delegating to its manager dependencies, and publishing completion events.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `servicedirector/sdapp.go` and `servicedirector/sdconfig.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **Configuration (`sdconfig.go`):** Implement the `Config` struct and the `ReadResourcesYAML` helper to parse the embedded `resources.yaml` file and discover the Pub/Sub topic and subscription IDs.
* **Application Logic (`sdapp.go`):**
    * **Construction:** Implement the `Director` struct. Use the Functional Options Pattern with a production constructor (`NewServiceDirector`) and a test constructor (`NewDirectServiceDirector`) that allows for dependency injection of the mock managers.
    * **Event Loop (`listenForCommands`):** Implement the core event loop. It must run in a background goroutine and continuously receive messages from the command subscription.
    * **Command Handling:** The event loop must unmarshal the incoming command message and use a `switch` statement on the `Command.Instruction` to call the appropriate handler method (e.g., `SetupFoundationalDataflow`).
    * **Delegation:** The handler methods (`SetupFoundationalDataflow`, etc.) must delegate the actual work by calling the methods on the `serviceManager` and `iamManager` dependencies.
    * **Event Publishing:** After a handler completes (either successfully or with an error), the service **must** publish a `CompletionEvent` to the completion topic.

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `servicedirector_integration_test.go`
2.  All `servicemanager` and `iam` interface and schema files.