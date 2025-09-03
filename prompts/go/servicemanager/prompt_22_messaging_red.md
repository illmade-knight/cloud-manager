# **Prompt 22: Unit & Integration Tests for Pub/Sub Adapter (RV_20250901_CORR3)**

**Objective:** Generate the complete test suite for the `googlemessaging.go` adapter, including both unit tests for internal logic and an integration test for the full lifecycle.

**TDD Phase:** Red (Generate failing tests before the code exists).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for two separate test files:
1.  **`servicemanager/googlemessaging_test.go`** (in package `servicemanager_test`): A **unit test** file using `testify/mock` to validate the adapter's internal type conversion and update logic.
2.  **`servicemanager/googlemessaging_integration_test.go`** (in package `servicemanager_test`): An **integration test** file that uses a Pub/Sub emulator to validate the full resource lifecycle.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing comprehensive and multi-layered test suites for Google Cloud services.

### **File 1: `googlemessaging_test.go` (Unit Tests)**
This file tests the adapter's internal logic in isolation. The package **must** be `servicemanager_test`.

* **Mocking:** Use the `testify/mock` library. The generated mock methods **must** match the real SDK signatures, including variadic `...gax.CallOption` parameters. Use the blank identifier `_` for any unused parameters in the mock's method body.
* **Test Coverage:**
    * **(L2-1.2) Type Conversion:** Write unit tests for the functions that convert between the generic `SubscriptionConfig` and the Google-specific `pubsubpb.Subscription`.
    * **Update Logic:** Write a unit test for the `Update` method of the subscription adapter. This test must mock the underlying `SubscriptionAdminClient` and assert that the generated `UpdateSubscriptionRequest` contains the correct `FieldMask` for the changes.

### **File 2: `googlemessaging_integration_test.go` (Integration Tests)**
This file tests the adapter's end-to-end functionality. The package **must** be `servicemanager_test`.

* **Environment:** The test **must not** use mocks. It must use an emulator.
* **Tagging:** The file **must** include the `//go:build integration` build tag.
* **Test Coverage:**
    * **(L1-2.1) Full Lifecycle:** Write a single, sequential test that validates the entire lifecycle: Create Topic -> Create Subscription -> Verify -> Delete Subscription -> Delete Topic -> Verify Deletion.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/messagingadapter.go`
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901_CORR3

This revision adds critical corrections regarding SDK method signatures and Go style conventions.

#### Clean Architectural Refinements
* **Adopted Hybrid Testing Strategy:** This prompt now generates both a unit test and an integration test to provide comprehensive coverage for the adapter's internal logic and its external interactions.

#### Anti-Pattern Corrections
* **Enforced Exact SDK Signatures:** The prompt now requires mock implementations to match the exact signatures of the Google Cloud SDK methods, including variadic `...gax.CallOption` parameters.
    * **Reason (Anti-Pattern Correction):** A previous generation produced mocks with incorrect method signatures, causing compilation failures. Mocks **must** be a precise reflection of the interface they are mocking.
* **Enforced Go Style Conventions:** The prompt now requires the use of the blank identifier `_` for unused parameters.
    * **Reason (Anti-Pattern Correction):** This is a standard Go convention for clean, lint-free code that must be followed in all generated code.