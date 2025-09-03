# **Prompt 03: Unit & Integration Tests for Cloud Run Adapter**

**Objective:** Generate the complete test suite for the `cloudrunadapter.go`. This includes a unit test for its internal logic and an integration test for the live deployment lifecycle.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for two separate test files in the `deployment_test` package:
1.  **`cloudrunadapter_test.go`**: A **unit test** using `testify/mock` to validate the adapter's internal logic, specifically the create-vs-update branching and the translation of the `DeploymentSpec`.
2.  **`cloudrunadapter_integration_test.go`**: An **integration test** that uses a live GCP environment to validate the full deployment, update, and deletion of a Cloud Run service.

## **Reinforcement Prompt (Detailed Context)**

### **File 1: `cloudrunadapter_test.go` (Unit Tests)**
This file tests the adapter's internal logic in isolation.

* **Package:** Must be `deployment_test`.
* **Mocking:** Use `testify/mock` to create a mock for the `*run.Service` client.
* **Test Coverage:**
    * **Create Path:** Write a test where the mock `Get` method returns a "not found" error. Assert that the `Create` method is then called.
    * **Update Path:** Write a test where the mock `Get` method returns an existing service. Assert that the `Patch` method is then called.
    * **(L2-1.2) Spec Translation:** In the above tests, use `mock.MatchedBy` to inspect the `*run.GoogleCloudRunV2Service` argument passed to `Create` or `Patch`. Assert that fields from our generic `DeploymentSpec` (like `Image`, `ServiceAccount`, `Env`, `Resources`) are correctly translated into the Google-specific struct.

### **File 2: `cloudrunadapter_integration_test.go` (Integration Tests)**
This file tests the adapter's end-to-end functionality.

* **Package:** Must be `deployment_test`.
* **Environment:** Runs against a live GCP API.
* **Tagging:** Must include the `//go:build integration` build tag.
* **Test Coverage:**
    * **(L1-2.3) Full Lifecycle:** Write a single, sequential test that:
        1. Deploys a new service using a public test image (e.g., `us-docker.pkg.dev/cloudrun/container/hello`).
        2. Polls the service until its "Ready" condition is `True`.
        3. Updates the service with a new configuration (e.g., a new environment variable).
        4. Verifies the update was applied.
        5. Deletes the service.
        6. Verifies the service is gone.

**Dependencies:**

1.  `deployment/cloudrunadapter.go`
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the Cloud Run adapter. It mandates the Hybrid Testing Strategy to ensure full coverage of the adapter's internal configuration logic and its live deployment lifecycle.