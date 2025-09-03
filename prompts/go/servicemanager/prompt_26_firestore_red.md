# **Prompt 26: Integration Test for Firestore Adapter (RV_20250901)**

**Objective:** Generate a complete integration test file for the Firestore adapter and its manager, including a test-only adapter to handle emulator limitations.

**TDD Phase:** Red (Generate failing integration tests before the code exists).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/firestoremanager_integration_test.go` file.

Write a complete integration test for the `FirestoreManager`. The test **must** define and use a **test-only adapter** to work around the Firestore emulator's known limitations with the Admin API.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust integration tests for Google Cloud services, and you are aware of the common quirks and limitations of the emulators.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/firestoremanager_integration_test.go`**.

**Key Requirements to Fulfill:**
* **Test Target:** The test **must** instantiate and test the `FirestoreManager`.
* **Environment:** The test must use a Firestore emulator.
* **Test-Only Adapter:** You **must** define the test-only adapter structs (implementing `DocumentStoreClient` and `DatabaseHandle`) **inside this test file**.
    * The `Exists()` method of the test handle must be implemented by attempting to create a data-plane client (`cloud.google.com/go/firestore`). If this succeeds, the database "exists".
    * The `Create()` and `Delete()` methods of the test handle must be implemented as no-ops that return `nil`.
* **Test Workflow:** The test **must** instantiate the `FirestoreManager` with the test-only adapter and then execute the full lifecycle:
    1.  Call `manager.CreateResources`.
    2.  Call `manager.Verify` and assert that it passes.
    3.  Call `manager.Teardown`.
    4.  Call `manager.Verify` *again* and assert that it still passes, confirming the database was not deleted.
* **Tagging:** The file **must** include the `//go:build integration` build tag.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/documentstore_adapter.go`
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This revision refines the integration test specification based on the robust patterns in the production code.

#### Clean Architectural Refinements
* **In-File Test Adapter:** The prompt now explicitly requires the test-only adapter to be defined within the integration test file itself.
    * **Reason:** This is a clean pattern that co-locates the test-specific workaround with the test that uses it, improving maintainability.
* **Manager-Level Testing:** The prompt clarifies that the test should target the `FirestoreManager`, not just the adapter.
    * **Reason:** This provides a more valuable end-to-end validation of the entire component's logic against the emulated service.