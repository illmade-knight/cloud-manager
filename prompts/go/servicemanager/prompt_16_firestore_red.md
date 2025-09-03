# **Prompt 16: Unit Tests for FirestoreManager (RV_20250901_CORR1)**

**Objective:** Generate the complete unit test file for the FirestoreManager. These tests will serve as the executable specification for the FirestoreManager's implementation.

**TDD Phase:** Red (Generate failing tests before the code exists).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/firestoremanager_test.go` file.

Using the provided `DocumentStoreClient` interface, write a comprehensive suite of unit tests for a `FirestoreManager` struct that does not yet exist. The tests **must** use mocks to validate logic in isolation.

**Key Requirements to Test:**
* **(L2-1.3) Idempotency:** The test must cover creating a new database and skipping an existing one.
* **(L2-2.1) Special Teardown Logic:** The `Teardown` test must verify that the manager does **not** attempt to delete the database and that no client methods are called.
* **Correct Naming:** The tests must assert that the client is asked for a database handle using the **logical name provided in the configuration** (e.g., "test-db").

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough unit tests using the testify suite (mock, require, assert).

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/firestoremanager_test.go`**.

**Key Requirements to Fulfill:**
* **Use Mocks:** You **must** use `testify/mock` to create mocks for `DocumentStoreClient` and `DatabaseHandle`.
* **Test Coverage:** You **must** create test cases for the following scenarios:
    * **`CreateResources`**:
        * A success case where a new database is created. The test must assert that `client.Database()` is called with the logical name from the config (e.g., "test-db"), and that `handle.Create()` is called with the correct `locationID` and `dbType`.
        * A success case where the database already exists and creation is skipped.
    * **`Teardown`**:
        * A test to confirm that `Teardown` is a no-op. It should execute without error and **must not** call any methods on the mock client.
    * **`Verify`**:
        * A success case where the database exists, validating that `client.Database()` is called with the logical name from the config.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/documentstore_adapter.go`
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901_CORR1

This revision **corrects a major flaw** identified during the refinement session. Previous prompts were based on a bug in the production code that incorrectly assumed a hardcoded Firestore database name.

#### Anti-Pattern Corrections
* **Corrected Hardcoded Name Bug:** The previous prompt revision incorrectly mandated testing for a hardcoded `"(default)"` database name. This was based on a flawed production code example.
    * **Reason (Anti-Pattern Correction):** GCP now supports multiple named Firestore instances. Hardcoding the name is a bug that limits functionality. The prompts **must** generate code that correctly uses the user-provided logical name from the configuration file. This ensures the generated code is more correct than the original production code it was compared against.