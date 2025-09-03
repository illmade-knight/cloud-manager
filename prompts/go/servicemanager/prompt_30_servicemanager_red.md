# **Prompt 30: Unit Tests for ServiceManager (RV_20250901)**

**Objective:** Generate a complete unit test file for the `ServiceManager`. These tests will validate the top-level orchestration logic.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/servicemanager_test.go` file.

Write a comprehensive suite of unit tests for a `ServiceManager` struct that does not yet exist. The tests **must** use mocks for the sub-manager interfaces (e.g., `IStorageManager`, `IMessagingManager`) to validate the orchestration logic in isolation.

**Key Requirements to Test:**
* **Delegation:** `SetupAll` tests must verify that the `CreateResources` method is called on the correct sub-managers for each dataflow.
* **Lifecycle Policy:** `TeardownAll` tests must verify that the `Teardown` method is only called for dataflows with an "ephemeral" lifecycle strategy.
* **Error Aggregation:** Tests must verify that errors from concurrent sub-manager operations are correctly collected and returned.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough unit tests for complex, concurrent systems using the `testify/mock` library.

**Your Specific Task:** Generate the complete code for `servicemanager/servicemanager_test.go`.

**Key Requirements to Fulfill:**
* **Use Mocks:** You **must** use `testify/mock` to create mock implementations for all the sub-manager interfaces (`IMessagingManager`, `IStorageManager`, etc.).
* **Test Setup:** Create a helper function (`setupServiceManagerTest`) that instantiates the `ServiceManager` using the `NewServiceManagerFromManagers` constructor, injecting the mocks. This helper should also create a sample `MicroserviceArchitecture` with multiple dataflows, including one `ephemeral` and one `permanent`.
* **Test Coverage:** You **must** create test cases for:
    * **`SetupAll`**:
        * A success case asserting that `CreateResources` is called on the appropriate mocks for each dataflow.
        * A failure case where one sub-manager returns an error, asserting that the top-level error from `SetupAll` contains the underlying error.
    * **`TeardownAll`**:
        * A success case asserting that `Teardown` is called **only** for the resources in the `ephemeral` dataflow.
        * The test **must assert** that `Teardown` is **not** called for resources in the `permanent` dataflow.
    * **`VerifyDataflow`**:
        * A success case where all sub-managers return `nil`.
        * A failure case where one sub-manager returns an error.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/cloudmanager.go` (contains the `IManager` interfaces to be mocked)
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `ServiceManager` unit tests, created based on the patterns established during the refinement of other manager prompts.