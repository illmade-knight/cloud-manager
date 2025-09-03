# **Prompt 01: Integration Test for ServiceDirector**

**Objective:** Generate a complete integration test for the `ServiceDirector`. This test will validate the Pub/Sub command handling and event publishing logic.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicedirector_integration_test.go` file.

Write an integration test for a `ServiceDirector` that does not yet exist. The test **must** use a Pub/Sub emulator to simulate the messaging layer and **mocks** for the `IServiceManager` and `IAMManager` dependencies. The test must verify that the service correctly receives a command, calls the appropriate mock manager method, and publishes a completion event.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `servicedirector_integration_test.go`.

**Key Requirements to Fulfill:**

* **Package:** The test file must be in the `servicedirector_test` package.
* **Environment:** The test **must** use a Pub/Sub emulator.
* **Tagging:** The file **must** include the `//go:build integration` build tag.
* **Mock Dependencies:** You **must** use `testify/mock` to create mock implementations of the `IServiceManager` and `IAMManager` interfaces.
* **Test Coverage:** The test must validate the end-to-end command-handling loop:
    1.  Set up the emulator with "command" and "completion" topics.
    2.  Instantiate the `ServiceDirector` with the **mocked** managers.
    3.  Start the `ServiceDirector` in a goroutine and wait for its `Ready()` channel to close.
    4.  Act as a test client: publish a command message to the command topic.
    5.  Act as a test listener: subscribe to the completion topic and wait for the reply.
    6.  Assert that the `ServiceDirector` called the correct method on the mocked manager (e.g., `mockServiceManager.On("SetupFoundationalDataflow", ...)`).
    7.  Assert that the correct `CompletionEvent` is received on the completion topic.

**Dependencies (Full source code to be provided):**

1.  `microservice/servicedirector/sdapp.go`
2.  All `servicemanager` and `iam` interface files.

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `ServiceDirector` integration test. It specifies a unique testing strategy that combines a live messaging emulator with mocked business logic dependencies.