# **Prompt 05: Implementation for RemoteDirectorClient**

**Objective:** Generate the implementation for the `RemoteDirectorClient`. The generated code must satisfy the previously defined integration test.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/directorclient.go` file.

I have provided a complete integration test file. Your implementation of the `RemoteDirectorClient` must make the tests pass by correctly implementing the asynchronous, Pub/Sub-based request/reply workflow.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust and correct asynchronous, event-driven systems using Go channels and goroutines.

**Your Specific Task:** Generate the complete code for `orchestration/directorclient.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **Initialization Logic:** The `NewRemoteDirectorClient` constructor and its `initialize` helper **must**:
    1.  Create a publisher for the command topic.
    2.  Create a new, unique Pub/Sub subscription on the completion topic. This is critical for ensuring that this specific client instance only receives replies intended for it.
* **(L2-1.4) Event Listener Goroutine:** The implementation **must** start a background goroutine (`listenForEvents`) that continuously receives messages from the unique completion subscription.
* **Asynchronous Wait Logic:** The core `sendCommandAndWait` helper method **must**:
    1.  Create a new `chan CompletionEvent` (the "waiter").
    2.  Store this channel in a thread-safe map, keyed by a unique identifier.
    3.  Publish the command message to the command topic.
    4.  Block and wait to receive a value from the waiter channel.
    5.  The `listenForEvents` goroutine is responsible for reading the event from Pub/Sub, finding the correct waiter channel in the map, and sending the event to it.
    6.  The logic must include a timeout to prevent the client from waiting indefinitely.
* **Cleanup (`Teardown`):** The `Teardown` method **must** correctly delete the unique Pub/Sub subscription that was created during initialization to avoid leaving orphaned resources.

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `orchestration/directorclient_integration_test.go`
2.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `RemoteDirectorClient` implementation. It requires a robust, asynchronous, event-driven implementation that correctly manages state using a waiter map and cleans up its own cloud resources.