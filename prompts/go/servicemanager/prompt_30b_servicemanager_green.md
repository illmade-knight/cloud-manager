# **Prompt 05: Implementation for ServiceManager (RV_20250901)**

**Objective:** Generate the complete implementation for the `ServiceManager`. The generated code must satisfy the previously defined unit tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/servicemanager.go` file.

I have provided the sub-manager interfaces and a complete unit test file (`servicemanager_test.go`). Your implementation of the `ServiceManager` **must** make all the tests in the provided test file pass.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing concurrent, robust, and production-grade orchestration logic.

**Your Specific Task:** Generate the complete code for `servicemanager/servicemanager.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement.
* **Constructors:** Implement three constructors:
    1.  `NewServiceManager`: A production constructor that scans the architecture and creates real Google Cloud clients for the required managers.
    2.  `NewServiceManagerFromClients`: A constructor for integration tests that accepts pre-configured client adapters.
    3.  `NewServiceManagerFromManagers`: A constructor for unit tests that accepts mock manager interfaces.
* **Orchestration Logic:**
    * **`SetupAll` / `SetupFoundationalDataflow`**: Implement the logic to iterate through dataflows and concurrently call the `CreateResources` method of each relevant sub-manager. You **must** use a `sync.WaitGroup` and a mutex-protected struct to safely aggregate the results and errors from the concurrent operations.
    * **`TeardownAll` / `TeardownDataflow`**: Implement the logic to check each dataflow's `Lifecycle.Strategy`. The `Teardown` method of the sub-managers **must only** be called if the strategy is `ephemeral`.
    * **`VerifyDataflow`**: Implement concurrent calls to each sub-manager's `Verify` method.

**Dependencies (Full source code to be provided):**

1.  **The Contract:** `servicemanager/cloudmanager.go`
2.  **The Specification:** `servicemanager/servicemanager_test.go`
3.  **The Schema:** `servicemanager/systemarchitecture.go`
4.  All manager and adapter implementation files (as the production constructor needs to call their factory functions).

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `ServiceManager` implementation. It incorporates all the "Clean Architectural Refinements" and "Anti-Pattern Corrections" learned from the previous manager refinement sessions, such as:
* Using multiple constructors for testability.
* Using the robust `WaitGroup` + channels/mutex pattern for concurrency.
* Enforcing specific business logic (like checking the lifecycle strategy).