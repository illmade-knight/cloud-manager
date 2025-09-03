# **Prompt 14: Unit Tests for BigQueryManager (RV_20250901)**

**Objective:** Generate a complete **unit test** file (`bigquery_test.go`) for the BigQueryManager. These tests will serve as the executable specification for the manager's internal logic.

**TDD Phase:** Red (Generate failing tests before the code exists).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/bigquery_test.go` file.

Using the `testify/mock` library, write a comprehensive suite of unit tests for a `BigQueryManager` struct that does not yet exist. The tests must validate the manager's internal logic, including schema validation, resource creation, and teardown logic.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough unit tests using `testify/mock`.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/bigquery_test.go`**.

**Key Requirements to Fulfill:**
* **Use `testify/mock`:** You must use this library for all mocking.
* **Test Setup:** Use a `TestMain` function to globally register a sample schema for use across all tests in the package.
* **Test Coverage:** You **must** create test cases for the following scenarios:
    * **`Validate` method**:
        * A success case where all schemas are registered and names are valid.
        * A failure case where a table's `SchemaType` is not found in the registry.
    * **`CreateResources`**:
        * A success case where new datasets and tables are created idempotently.
        * A partial failure case where one resource fails to create.
    * **`Teardown`**:
        * A success case where `dataset.DeleteWithContents()` is called for unprotected datasets.
        * The test **must assert** that no individual table deletion methods are called.
        * A case where a dataset with `TeardownProtection` is correctly skipped.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/bigqueryadapter.go`
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This revision updates the test specification to enforce more robust, production-ready patterns observed in the committed codebase.

#### Clean Architectural Refinements
* **Idempotency Check:** The validation logic was changed to mock `handle.Metadata()` or `handle.Attrs()` and check for a generic "not found" error.
    * **Reason:** This is a more efficient and robust pattern, better fulfilling **L2-1.3 (Idempotency)**.
* **Constructor Validation:** Added a required test case for manager constructors to ensure they return an error when a client dependency is `nil`.
    * **Reason:** Enforces robust instantiation, contributing to overall system reliability.
* **Specific Teardown Failure:** Added test cases for common, provider-specific failure modes (e.g., "bucket not empty").
    * **Reason:** Ensures the manager provides clear, user-friendly errors for actionable failures, improving resiliency as per **L2-2.3**.
* **Partial Failure Return Value:** Clarified assertions for partial failure cases to ensure only successfully provisioned resources are returned.
    * **Reason:** Ensures the implementation correctly handles concurrent operations as required by **L2-1.4 (Concurrency)**.
* **Pre-flight Validation:** Added required test cases to ensure a client's or manager's `Validate` method is called before resource creation.
    * **Reason:** Enforces a fail-fast approach, contributing to **L2-2.3 (Resiliency)**.
* **Idempotent Teardown:** Added test cases to ensure that "not found" errors are ignored during teardown.
    * **Reason:** This makes the teardown operation idempotent, directly fulfilling **L2-1.3 (Idempotency)**.
* **Internal Validation (BigQuery):** The test spec now requires a dedicated test suite for the `BigQueryManager`'s internal `Validate` method.
    * **Reason:** This ensures the manager's prerequisite checks (like schema registration) are thoroughly tested.
* **Efficient Teardown Test (BigQuery):** The teardown test was made more specific to enforce the dataset-level deletion strategy.
    * **Reason:** This ensures the generated code uses the most efficient teardown method provided by the API, fulfilling **L1-2.1 (Infrastructure Provisioning)**.