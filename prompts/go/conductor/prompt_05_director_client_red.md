# **Prompt 05: Integration Test for RemoteDirectorClient**

**Objective:** Generate a complete integration test for the `RemoteDirectorClient`. This test will validate the asynchronous, Pub/Sub-based request/reply workflow using an emulator.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/directorclient_integration_test.go` file.

Write a complete integration test for a `RemoteDirectorClient` that does not yet exist. The test **must** use a Pub/Sub emulator and include a lightweight, test-local "mock ServiceDirector" to simulate the remote service. The test must verify that the client can send a command and correctly receive the asynchronous completion event.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing robust integration tests for complex, event-driven, and asynchronous messaging systems.

**Your Specific Task:** Generate the complete code for `orchestration/directorclient_integration_test.go`.

**Key Requirements to Fulfill:**

* **Package:** The test file must be in the `orchestration_test` package.
* **Environment:** The test **must** use a Pub/Sub emulator to create a realistic messaging environment.
* **Tagging:** The file **must** include the `//go:build integration` build tag.
* **Mock ServiceDirector:** The test file **must** include a `mockServiceDirector` struct. This is not a `testify` mock, but a simulator that:
    1.  Subscribes to the "command" topic on the emulator.
    2.  Runs a background goroutine to listen for incoming command messages.
    3.  When a command is received, it immediately publishes a corresponding `CompletionEvent` message to the "completion" topic.
* **Test Coverage:** The test must validate the end-to-end communication flow:
    1.  Set up the emulator and the `mockServiceDirector`.
    2.  Instantiate the real `RemoteDirectorClient` (the component under test).
    3.  Call a method on the client, such as `TriggerFoundationalSetup`.
    4.  Assert that the method blocks and eventually returns the correct `CompletionEvent` that was published by the `mockServiceDirector`.
    5.  The test must ensure that all Pub/Sub resources, including the client's listener subscription, are cleaned up.

**Dependencies (Full source code to be provided):**

1.  `orchestration/directorclient.go`
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `RemoteDirectorClient`. It specifies an integration test with a test-local service simulator to validate the component's complex, asynchronous communication pattern.