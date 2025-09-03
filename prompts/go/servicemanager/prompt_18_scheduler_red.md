# **Prompt 18: Unit Tests for SchedulerManager (RV_20250901)**

**Objective:** Generate the complete unit test file for the CloudSchedulerManager. These tests will serve as the executable specification for the implementation.

**TDD Phase:** Red (Generate failing tests before the code exists).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/schedulermanager_test.go` file.

Write a comprehensive suite of unit tests for a `CloudSchedulerManager` struct that does not yet exist. The tests **must use the `testify/mock` library** to validate logic in isolation.

**Key Requirements to Test:**
* **(L2-2.5) Validation:** The test must verify that the manager calls the client's `Validate` method before creating any jobs.
* **(L2-1.3) Idempotency:** The `CreateResources` test must cover creating a new job and skipping an existing one. The `Teardown` test must ignore "not found" errors.
* **(L2-2.1) Lifecycle Protection:** The `Teardown` test must verify that a protected job is not deleted.
* **(L1-2.1) Verification:** A full test suite for the `Verify` method must be included.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough unit tests using the `testify` suite, especially `testify/mock`.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/schedulermanager_test.go`**.

**Key Requirements to Fulfill:**
* **Use `testify/mock`:** You **must** use the `testify/mock` library to create mock structs for `SchedulerClient` and `SchedulerJob`. You **must not** use hand-rolled mock structs.
* **Test in Parallel:** All top-level test functions (`Test...`) should be marked with `t.Parallel()` if they are independent.
* **Test Coverage:** You **must** create complete test suites for `CreateResources`, `Teardown`, and `Verify`, covering the following scenarios:
    * **`CreateResources`**:
        * A failure case where `client.Validate()` returns an error.
        * Success case for creating a new job.
        * Success case where a job already exists and creation is skipped.
    * **`Teardown`**:
        * Success case where unprotected jobs are deleted.
        * A case where `TeardownProtection` is enabled and the job is correctly skipped.
        * A case where `Delete()` returns a "not found" error, ensuring the overall operation succeeds.
    * **`Verify`**:
        * Success case where all jobs are found.
        * A failure case where a job is not found.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/scheduleradapter.go` (contains the interfaces to be mocked)
2.  `servicemanager/systemarchitecture.go` (contains the config structs)

---

## Prompt Changelog

### RV_20250901

This revision aligns the test specification with the project-wide decision to standardize on the `testify/mock` library and adds missing test cases.

#### Clean Architectural Refinements
* **Added `Verify` Method Tests:** The original prompt was missing a test suite for the `Verify` method. This has been added to ensure full coverage of the manager's interface, fulfilling **L1-2.1 (Infrastructure Provisioning)**.
* **Idempotent Teardown:** Added a test case to ensure "not found" errors are ignored during teardown, fulfilling **L2-1.3 (Idempotency)**.

#### Anti-Pattern Corrections
* **Mocking Strategy:** The mocking strategy was changed from hand-rolled mock structs to the **`testify/mock` library**.
    * **Reason (Anti-Pattern Correction):** A project-wide architectural decision was made to standardize on a single mocking strategy. `testify/mock` is already used by other managers and provides a more declarative and maintainable approach that is consistent with the project's TDD philosophy. This change corrects the stylistic inconsistency found in the initial production code.