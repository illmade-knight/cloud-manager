# **Prompt 04: Integration Test for IAMOrchestrator**

**Objective:** Generate a complete integration test for the `IAMOrchestrator`. This test will validate the orchestrator's ability to correctly sequence SA creation, policy application, and verification polling against a live GCP environment.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/iamorchestrator_integration_test.go` file.

Write a complete integration test for an `IAMOrchestrator` that does not yet exist. The test **must not** use mocks. It must use a live Google Cloud environment to verify the entire IAM setup lifecycle, including creating Service Accounts, applying policies, and polling for propagation.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust integration tests for complex, stateful cloud security workflows.

**Your Specific Task:** Generate the complete code for `orchestration/iamorchestrator_integration_test.go`.

**Key Requirements to Fulfill:**

* **Package:** The test file must be in the `orchestration_test` package.
* **Environment:** The test must use real GCP credentials and read the project ID from an environment variable.
* **Tagging:** The file **must** include the `//go:build integration` build tag.
* **(L1-1.2) Test Full Lifecycle Sequence:** The test must validate the entire IAM setup workflow in the correct sequence:
    1.  Instantiate the `IAMOrchestrator`.
    2.  Call `EnsureDataflowSAsExist` to create service accounts for a test architecture.
    3.  Call `PollForSAExistence` to wait for the newly created SAs to become available.
    4.  Call `ApplyProjectLevelIAMForDataflow` to grant roles.
    5.  Call `VerifyProjectLevelIAMForDataflow` to poll and confirm the roles have propagated successfully.
* **Cleanup:** The test **must** use `t.Cleanup` to call the orchestrator's `Teardown` method, ensuring all created service accounts are deleted after the test run.

**Dependencies (Full source code to be provided):**

1.  `orchestration/iamorchestrator.go`
2.  `iam` package files (interfaces and clients)
3.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `IAMOrchestrator` integration test. It focuses on validating the correct sequencing of operations and the orchestrator's ability to handle the eventual consistency of cloud IAM APIs.