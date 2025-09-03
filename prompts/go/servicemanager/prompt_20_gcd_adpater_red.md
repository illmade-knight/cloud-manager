# **Prompt 20: Unit & Integration Tests for GCS Adapter (RV_20250901)**

**Objective:** Generate the complete test suite for the `googlegcs.go` adapter, including both unit tests for internal logic and an integration test for the full lifecycle.

**TDD Phase:** Red (Generate failing tests before the code exists).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for two separate test files:
1.  **`servicemanager/googlegcs_test.go`**: A **unit test** file using `testify/mock` to validate the adapter's internal type conversion logic.
2.  **`servicemanager/googlegcs_integration_test.go`**: An **integration test** file that uses a GCS emulator to validate the full resource lifecycle.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing comprehensive and multi-layered test suites for cloud services.

**Your Specific Task:** Your task is to generate the complete code for the two test files described below.

### **File 1: `googlegcs_test.go` (Unit Tests)**
This file tests the adapter's internal logic in isolation.

* **Mocking:** Use the `testify/mock` library.
* **Test Coverage:**
    * **(L2-1.2) Type Conversion:** Write table-driven tests for the `fromGCSBucketAttrs` and `toGCSBucketAttrs` helper functions to ensure they are correct.
    * **Update Logic:** Write a unit test for the `gcsBucketHandleAdapter.Update` method to verify its complex logic for calculating label diffs.

### **File 2: `googlegcs_integration_test.go` (Integration Tests)**
This file tests the adapter's end-to-end functionality against a real service.

* **Environment:** The test **must not** use mocks. It must programmatically start a GCS emulator using the provided emulator package.
* **Tagging:** The file **must** include the `//go:build integration` build tag.
* **Test Coverage:**
    * **(L1-2.1) Full Lifecycle:** Write a single, sequential test that validates the entire lifecycle: Create -> Verify -> Update -> Verify Update -> Delete -> Verify Delete.
    * **Error Handling:** The test must verify that attempting to get attributes for a deleted bucket returns the generic `servicemanager.ErrBucketNotExist` error.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/storageadapter.go` (contains the `StorageClient` interface)
2.  `servicemanager/systemarchitecture.go` (contains the `GCSBucket` struct)

---

## Prompt Changelog

### RV_20250901

This revision implements the new Hybrid Testing Strategy and standardizes the mocking library.

#### Clean Architectural Refinements
* **Adopted Hybrid Testing Strategy:** This prompt now generates both a unit test and an integration test.
    * **Reason:** This provides more comprehensive coverage as per our updated plan. Unit tests are better for validating complex internal logic like type conversions, while integration tests validate the end-to-end lifecycle against a real service.

#### Anti-Pattern Corrections
* **Mocking Strategy:** The mocking strategy for unit tests was standardized to `testify/mock`.
    * **Reason (Anti-Pattern Correction):** This corrects the stylistic inconsistency of using hand-rolled mocks in the original production code and aligns the adapter tests with the project-wide standard.