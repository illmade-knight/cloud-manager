# **Prompt 10: Unit Tests for StorageManager (RV_20250901)**

**Objective:** Generate the complete unit test file for the StorageManager. These tests will serve as the executable specification for the StorageManager's implementation.

**TDD Phase:** Red (Generate failing tests before the code exists).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/storagemanager_test.go` file.

Using the provided `StorageClient` interface, write a comprehensive suite of unit tests for a `StorageManager` struct that does not yet exist. The tests **must** use mocks to validate all logic in isolation.

**Key Requirements to Test:**
* **(L1-2.1) Infrastructure Provisioning:** Test the full lifecycle: `CreateResources`, `Verify`, and `Teardown`.
* **(L2-1.3) Idempotency:** The `CreateResources` test must cover both creating a new bucket and updating an existing one, based on the error returned from `Attrs`.
* **(L2-1.4) Concurrency:** The tests should be structured to handle multiple resources in the input spec.
* **(L2-2.1) Lifecycle Protection:** The `Teardown` test must verify that a bucket with `teardown_protection: true` is not deleted.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough unit tests using the testify suite (mock, require, assert).

**Overall Goal:** We are building the `servicemanager` package using a TDD approach. Our next step is to write the tests that will define and validate the behavior of the `StorageManager`.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/storagemanager_test.go`**. This file will contain a full suite of unit tests for the `StorageManager`.

**Key Requirements to Fulfill:**
* **Use Mocks:** You **must** use `testify/mock` to create a `MockStorageClient` that implements the `StorageClient` interface.
* **Test Coverage:** You **must** create test cases for the following scenarios:
    * **`NewStorageManager`**:
        * A failure case where the client dependency is `nil`.
    * **`CreateResources`**:
        * Success case where all buckets are new and created successfully.
        * Success case where all buckets already exist and are updated successfully.
        * Partial failure case where one of several buckets fails to be created. The test **must assert** that the returned slice of provisioned resources contains *only* the successfully created bucket.
    * **`Teardown`**:
        * Success case where all buckets are deleted.
        * A case where a bucket with `TeardownProtection` enabled is correctly skipped.
        * A failure case where `handle.Delete()` returns an error containing the string "not empty", and assert that the manager returns a more user-friendly, wrapped error message.
    * **`Verify`**:
        * Success case where all buckets are found.
        * Failure case where one of several buckets is not found.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/storageadapter.go` (contains the `StorageClient` interface to be mocked)
2.  `servicemanager/systemarchitecture.go` (contains the `GCSBucket` struct)

---

## Prompt Changelog

### RV_20250901

This revision updates the test specification to enforce more robust, production-ready patterns.

#### Clean Architectural Refinements
* **Idempotency Check:** The validation logic was changed to mock `handle.Attrs()` and check for `servicemanager.ErrBucketNotExist`.
    * **Reason:** This is a more efficient and robust pattern, better fulfilling **L2-1.3 (Idempotency)**.
* **Constructor Validation:** Added a required test case for the `NewStorageManager` constructor.
    * **Reason:** Enforces robust instantiation, contributing to overall system reliability.
* **Specific Teardown Failure:** Added a test case for the "bucket not empty" failure.
    * **Reason:** Ensures the manager provides a clear error for common failures, improving resiliency as per **L2-2.3**.
* **Partial Failure Return Value:** Clarified the assertion for the partial failure case.
    * **Reason:** Ensures the implementation correctly handles concurrent operations as required by **L2-1.4 (Concurrency)**.