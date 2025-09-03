# **Prompt 04: Unit & Integration Tests for CloudBuildDeployer**

**Objective:** Generate the complete test suite for the `CloudBuildDeployer`. This includes a unit test for its orchestration logic and an integration test for the full "source-to-URL" pipeline.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for two separate test files in the `deployment_test` package:
1.  **`cloudbuilddeployer_test.go`**: A **unit test** using `testify/mock` to validate the `CloudBuildDeployer`'s orchestration logic in isolation.
2.  **`cloudbuilddeployer_integration_test.go`**: An **integration test** that uses live GCP services to validate the full, end-to-end build and deploy pipeline.

## **Reinforcement Prompt (Detailed Context)**

### **File 1: `cloudbuilddeployer_test.go` (Unit Tests)**
This file tests the orchestrator's logic without making any real cloud calls.

* **Package:** Must be `deployment_test`.
* **Mocking:** Use `testify/mock` to create mocks for the **internal interfaces** (`storageAPI`, `artifactRegistryAPI`, `cloudBuildAPI`, `cloudRunAPI`).
* **Test Coverage:**
    * **(L1-2.3) Full Success Path:** Write a test for a successful `BuildAndDeploy` call. The test must set up expectations on all the mock interfaces in the correct sequence (Ensure Bucket -> Ensure Repo -> Upload -> Trigger Build -> Create/Update Service) and assert that all expectations are met.
    * **Partial Failure Path:** Write a test where an intermediate step fails (e.g., the mock `cloudBuildAPI` returns an error). Assert that the process stops and the subsequent steps (e.g., calling the `cloudRunAPI`) are not executed.
    * **`Teardown`:** Write a test for the `Teardown` method and assert that it correctly delegates the call to the mock `cloudRunAPI.Delete` method.

### **File 2: `cloudbuilddeployer_integration_test.go` (Integration Tests)**
This file tests the full pipeline against live GCP services.

* **Package:** Must be `deployment_test`.
* **Environment:** Runs against a live GCP API. Must check for necessary environment variables like `GCP_PROJECT_ID`.
* **Tagging:** Must include the `//go:build integration` build tag.
* **Test Coverage:**
    * **(L1-2.3) End-to-End Pipeline:** Write a single, comprehensive test that:
        1.  Programmatically creates temporary source code for a simple "hello world" Go application.
        2.  Instantiates the real `CloudBuildDeployer`.
        3.  Calls the `BuildAndDeploy` method.
        4.  Uses a polling mechanism (`require.Eventually`) to connect to the live Cloud Run API and verify that the new service revision becomes "Ready".
        5.  Includes a full `t.Cleanup` process to call the `Teardown` method and ensure the deployed Cloud Run service is deleted.

**Dependencies:**

1.  `deployment/internal_adapters.go` (contains the interfaces to be mocked)
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `CloudBuildDeployer`. It mandates the Hybrid Testing Strategy to ensure full coverage of the complex, multi-service orchestration pipeline.