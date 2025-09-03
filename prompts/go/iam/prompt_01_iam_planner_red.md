# **Prompt 01: Unit Tests for IAM RolePlanner**

**Objective:** Generate the complete unit test file for the `RolePlanner`. This test will act as the executable specification for the core IAM planning logic.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/iamplanner_test.go` file.

Write a comprehensive, table-driven unit test for a `RolePlanner` that does not yet exist. The test must validate that the planner correctly translates a complex `MicroserviceArchitecture` configuration into a flat list of the correct `IAMBinding` structs for all services and resources.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough, table-driven unit tests.

**Your Specific Task:** Generate the complete code for `iam/iamplanner_test.go`.

**Key Requirements to Fulfill:**
* **No Mocks:** The `RolePlanner` is pure logic, so this test **must not** use any mocks.
* **Comprehensive Test Architecture:** The test setup must create a detailed `MicroserviceArchitecture` struct that includes configurations for a wide variety of resources and service interactions, including:
    * Pub/Sub topics and subscriptions with producers/consumers.
    * GCS buckets with producers/consumers.
    * BigQuery tables with consumers.
    * Firestore databases with producers/consumers.
    * Services with service-to-service `dependencies`.
    * Services that use `secret_environment_vars`.
* **Sub-Test Assertions:** The test must use sub-tests (`t.Run`) to validate the generated IAM plan for each service account individually.
* **Helper Function:** Include a test-local helper function (e.g., `findBindingsForSA`) to filter the resulting plan for a specific service account, making assertions cleaner.

**Dependencies (Full source code to be provided):**

1.  `iam/iamplanner.go` (will contain the `IAMBinding` struct)
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `RolePlanner` unit tests, created based on the patterns established during the refinement of the `servicemanager` package.