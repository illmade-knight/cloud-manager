# **Prompt 11: Implementation for StorageManager (RV_20250901)**

**Objective:** Generate the complete implementation for the `StorageManager`. The generated code must satisfy the previously defined unit tests.

**TDD Phase:** Green (Write the code to make the failing tests pass).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/storagemanager.go` file.

I have provided the `StorageClient` interface that the manager must use, and a complete unit test file (`storagemanager_test.go`). Your implementation of the `StorageManager` **must** make all the tests in the provided test file pass. The implementation must define and use a generic `ErrBucketNotExist` variable and use a `WaitGroup` and channels for concurrency.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing concurrent, robust, and production-grade cloud service clients.

**Overall Goal:** We are building the `servicemanager` package using a Test-Driven Development approach. We have already written the unit tests that define the required behavior of the `StorageManager`. Your task is now to write the implementation code that satisfies those tests.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/storagemanager.go`**. This file will contain the `StorageManager` struct and its methods. The logic you write must be correct according to the provided test specification.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement. Your generated code must correctly implement all the logic necessary to make the tests in the provided `storagemanager_test.go` file pass. This includes:
    * **Generic Error Handling:** The file **must** define a public, package-level `var ErrBucketNotExist = errors.New("storage: bucket does not exist")`.
    * **Constructor Validation**: The `NewStorageManager` constructor **must** check if the provided client is `nil` and return an error if it is.
    * **(L2-1.3) Idempotent Creation:** In `CreateResources`, you **must** implement the following logic: first, get a bucket handle from the client; second, call the handle's `Attrs` method. You **must** then use `errors.Is(err, ErrBucketNotExist)` to determine if the bucket exists. If it does, call `Update`; if it doesn't, call `Create`.
    * **(L2-1.4) Concurrency:** Use a `sync.WaitGroup` and two separate channels: one for collecting errors (`chan error`) and one for collecting successfully provisioned resources (`chan ProvisionedGCSBucket`). This allows you to return partial results on failure.
    * **(L2-2.1) Teardown Protection:** In `Teardown`, check the `TeardownProtection` flag before attempting to delete a bucket.
    * **Specific Teardown Errors**: In `Teardown`, after receiving an error from `handle.Delete()`, check if the error string contains "not empty" and wrap it in a more user-friendly error message if it does.
    * **Verification:** The `Verify` method must correctly check for the existence of all specified buckets.

**Dependencies (Full source code to be provided):**

1.  **The Contract:** `servicemanager/storageadapter.go`
2.  **The Specification:** `servicemanager/storagemanager_test.go`
3.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This revision updates the implementation instructions to generate more robust, production-ready code that aligns with the refined test specification.

#### Clean Architectural Refinements
* **Idempotency Logic:** Specified the exact `handle.Attrs()` -> `errors.Is()` workflow.
    * **Reason:** Enforces a more efficient and robust pattern for checking resource existence, better fulfilling **L2-1.3 (Idempotency)**.
* **Constructor Validation:** Required the `NewStorageManager` constructor to validate its dependencies.
    * **Reason:** Prevents runtime panics from `nil` dependencies, contributing to overall system reliability.
* **Concurrency Pattern:** Changed the required concurrency pattern to `sync.WaitGroup` with dedicated channels.
    * **Reason (Anti-Pattern Correction):** The initial prompt was too generic. This new pattern is required to support the collection of partial successes, which better fulfills the spirit of **L2-1.4 (Concurrency)** in a multi-resource context.

#### Fixes for LLM/API Limitations
* **Brittle Error Parsing:** The prompt requires parsing a raw error string ("not empty") from the `Delete` method.
    * **Reason (Fix):** This is a pragmatic **fix** for a limitation where the underlying adapter might not return a typed error. This is a practical step toward fulfilling **L2-2.3 (Resiliency to Eventual Consistency)** by handling real-world API error responses gracefully.