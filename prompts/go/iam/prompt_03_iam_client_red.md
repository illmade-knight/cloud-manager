# **Prompt 03: Unit & Integration Tests for GoogleIAMClient**

**Objective:** Generate the complete test suite for the `GoogleIAMClient`. This includes unit tests for its complex, multi-service orchestration logic and an integration test to validate its behavior against live Google Cloud APIs.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate two test files in the `iam_test` package:
1.  **`googleiamclient_test.go`**: A **unit test** using `testify/mock` to validate the adapter's internal logic, such as the `switch` statement that delegates to different clients and the special additive logic for Cloud Run.
2.  **`googleiamclient_integration_test.go`**: An **integration test** that validates the full lifecycle (Apply, Check, Remove) of IAM policies on real, temporary Google Cloud resources.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing comprehensive, multi-layered test suites for complex cloud service clients.

### **File 1: `googleiamclient_test.go` (Unit Tests)**
This file tests the adapter's internal orchestration logic in isolation. The package **must** be `iam_test`.

* **Mocking:** The `GoogleIAMClient` will be instantiated with mocks for the underlying Google Cloud clients it depends on (e.g., a mock `*pubsub.Client`, a mock `*run.Service`).
* **Test Coverage:**
    * **(L2-1.2) `ApplyIAMPolicy` Delegation:** Write a test that uses a table-driven approach to verify the `switch` statement. For a given `ResourceType` (e.g., "pubsub_topic"), assert that the correct underlying client's `SetIamPolicy` method is called.
    * **(L2-2.2) Cloud Run Additive Logic:** Write a specific test for the "cloudrun_service" case. Assert that the mock `run.Service` has `GetIamPolicy` called *before* `SetIamPolicy` is called, validating the additive logic.
    * **Service Account Creation:** Test the `EnsureServiceAccountExists` method, covering both the case where the account already exists (`GetServiceAccount` succeeds) and the case where it doesn't (`GetServiceAccount` returns a "not found" error, and `CreateServiceAccount` is then called).

### **File 2: `googleiamclient_integration_test.go` (Integration Tests)**
This file tests the adapter's end-to-end functionality against live GCP APIs. The package **must** be `iam_test`.

* **Environment:** The test **must not** use mocks. It must use real GCP credentials.
* **Tagging:** The file **must** include the `//go:build integration` build tag.
* **Test Coverage:**
    * **(L1-2.2) Full IAM Lifecycle:** Write a test that performs the full Apply -> Check -> Remove lifecycle for at least two representative resource types (e.g., a Pub/Sub topic and a Cloud Run service).
    * **Prerequisite Resources:** The test must programmatically create temporary resources (e.g., a topic, a service) to apply policies to, and ensure they are cleaned up at the end of the test.
    * **(L2-2.3) Eventual Consistency:** The test **must** use a polling mechanism (e.g., `require.Eventually`) when checking for IAM policy propagation after an `Apply` or `Remove` operation.

**Dependencies (Full source code to be provided):**

1.  `iam/iamadapter.go`
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `GoogleIAMClient` adapter tests. It mandates the Hybrid Testing Strategy to ensure full coverage of this complex component.