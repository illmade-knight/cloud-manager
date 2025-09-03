# **Prompt 04: Unit & Integration Tests for GoogleIAMProjectClient**

**Objective:** Generate the complete test suite for the `GoogleIAMProjectClient`. This includes a unit test for its internal security checks and an integration test for its live API interactions.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate two test files in the `iam_test` package:
1.  **`googleprojectiam_test.go`**: A **unit test** using `testify/mock` to validate the security guardrail in the `GrantFirestoreProjectRole` method.
2.  **`googleprojectiam_integration_test.go`**: An **integration test** that uses a live GCP environment to validate the full "get-modify-set" lifecycle of a project-level IAM binding.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing multi-layered test suites for sensitive cloud security components.

### **File 1: `googleprojectiam_test.go` (Unit Tests)**
This file tests the client's internal logic in isolation. The package **must** be `iam_test`.

* **Mocking:** Use `testify/mock` to create a mock for the underlying `*resourcemanager.ProjectsClient`.
* **Test Coverage:**
    * **(L2-1.1) `GrantFirestoreProjectRole` Security Check:**
        * Write a test case that calls `GrantFirestoreProjectRole` with an invalid, non-datastore role (e.g., `roles/owner`). Assert that an error is returned and that the mock client's `SetIamPolicy` method is **never** called.
        * Write a test case that calls `GrantFirestoreProjectRole` with a valid role (e.g., `roles/datastore.user`). Assert that no error is returned and that the mock client's `SetIamPolicy` method **is** called.

### **File 2: `googleprojectiam_integration_test.go` (Integration Tests)**
This file tests the client's end-to-end functionality against a live GCP project. The package **must** be `iam_test`.

* **Environment:** The test **must not** use mocks. It must use real GCP credentials.
* **Tagging:** The file **must** include the `//go:build integration` build tag.
* **Test Coverage:**
    * **(L1-2.2) Full IAM Lifecycle:** Write a test that performs the full Add -> Check -> Remove lifecycle for a project-level role.
    * **Prerequisite Resources:** The test must programmatically create a temporary service account to be the `member` in the IAM binding. This service account must be cleaned up at the end of the test.
    * **(L2-2.3) Eventual Consistency:** The test **must** use a polling mechanism (e.g., `require.Eventually`) when checking for IAM policy propagation after adding or removing a binding.

**Dependencies (Full source code to be provided):**

1.  `iam/googleprojectiam.go` (will contain the `GoogleIAMProjectClient`)
2.  `iam/googleiamadmin.go` (for creating the prerequisite test service account)

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `GoogleIAMProjectClient`. It mandates a Hybrid Testing Strategy to test both the critical internal security logic and the live API interaction.