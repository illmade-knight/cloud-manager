# **Prompt 16: Implementation for FirestoreManager (RV_20250901_CORR1)**

**Objective:** Generate the complete implementation for the `FirestoreManager`. The generated code must satisfy the previously defined unit tests.

**TDD Phase:** Green (Write the code to make the failing tests pass).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/firestoremanager.go` file.

I have provided the `DocumentStoreClient` interface and a complete unit test file. Your implementation **must** make all the tests pass. The implementation must use the **logical name from the configuration** when getting a database handle, and the `Teardown` method must be a no-op.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing concurrent, robust, and production-grade cloud service clients.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/firestoremanager.go`**.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement. Your generated code must correctly implement all logic to make the tests pass. This includes:
    * **(L1-2.1) Correct Naming:** When calling `client.Database()`, you **must** use the `Name` field from the `FirestoreDatabase` configuration struct. Do not use a hardcoded name.
    * **(L2-1.3) Idempotent Creation:** In `CreateResources`, you must first get the database handle (using its correct name), call `handle.Exists()`, and only call `handle.Create()` if it does not exist. You must pass the `LocationID` and `Type` from the config to the `Create` method.
    * **(L2-2.1) Protected Teardown:** The `Teardown` method **must not** call any client methods. It should simply log a warning message.
    * **(L2-1.4) Concurrency:** Use a `sync.WaitGroup` and dedicated channels for errors and provisioned resources in `CreateResources`.

**Dependencies (Full source code to be provided):**

1.  **The Contract:** `servicemanager/documentstore_adapter.go`
2.  **The Specification:** `servicemanager/firestoremanager_test.go`
3.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901_CORR1

This revision **corrects a major flaw** identified during the refinement session. Previous prompts were based on a bug in the production code that incorrectly assumed a hardcoded Firestore database name.

#### Anti-Pattern Corrections
* **Corrected Hardcoded Name Bug:** The previous prompt revision incorrectly mandated implementing logic for a hardcoded `"(default)"` database name.
    * **Reason (Anti-Pattern Correction):** GCP now supports multiple named Firestore instances. The refined prompt now mandates that the implementation use the user-provided logical name from the configuration. This correction ensures our prompt-driven process produces code that is **more correct and feature-complete** than the original production code it was compared against, fully realizing the goal of the refinement phase.