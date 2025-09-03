# **Prompt 04: Implementation for IAMOrchestrator**

**Objective:** Generate the implementation for the `IAMOrchestrator`. The generated code must satisfy the previously defined integration test.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/iamorchestrator.go` file.

I have provided a complete integration test file. Your implementation of the `IAMOrchestrator` must make the tests pass by correctly managing the lifecycle of service accounts and IAM policies, including implementing robust polling logic to handle eventual consistency.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing concurrent, robust, and production-grade cloud security orchestration logic.

**Your Specific Task:** Generate the complete code for `orchestration/iamorchestrator.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **(L2-1.1) Orchestration Logic:** Implement the full suite of methods required by the test (`EnsureDataflowSAsExist`, `PollForSAExistence`, `ApplyProjectLevelIAMForDataflow`, `VerifyProjectLevelIAMForDataflow`, `Teardown`, etc.).
* **State Management:** The orchestrator struct **must** track the email addresses of all service accounts it creates, so that the `Teardown` method can correctly clean them up.
* **(L2-2.3) Polling for Propagation:** The methods that verify IAM state (`PollForSAExistence`, `Verify...IAM...`) **must** be implemented with a robust polling loop. This loop must include a configurable timeout and a fixed polling interval to correctly handle the eventual consistency of Google Cloud's IAM systems.
* **Dependency Usage:** The implementation must correctly use its dependencies:
    * The `iam.RolePlanner` to generate the list of required permissions.
    * The `iam.IAMClient` for creating service accounts and managing resource-level policies.
    * The `iam.GoogleIAMProjectClient` for managing project-level policies.

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `orchestration/iamorchestrator_integration_test.go`
2.  **The Contracts:** All interfaces from the `iam` package.
3.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `IAMOrchestrator` implementation. It requires a stateful orchestrator that can correctly sequence operations and handle the eventual consistency of cloud IAM through robust polling logic.