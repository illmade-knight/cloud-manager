# **Prompt 14: Implementation for BigQueryManager (RV_20250901)**

**Objective:** Generate the complete implementation for the `BigQueryManager` and its associated adapters. The generated code must satisfy the previously defined unit tests.

**TDD Phase:** Green (Write the code to make the failing tests pass).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/bigquery.go` file.

This single file should contain the `BigQueryManager`, the `BQClient` interface abstraction, and the concrete `bqClientAdapter` implementation that wraps the real Google Cloud BigQuery client. The implementation must make all tests in `bigquery_test.go` pass.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing concurrent, robust, and production-grade cloud service clients.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/bigquery.go`**.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement.
* **File Structure:** The single `bigquery.go` file must contain:
    1.  The `BQClient`, `BQDataset`, and `BQTable` interface definitions.
    2.  The adapter structs (e.g., `bqClientAdapter`) that implement those interfaces by wrapping the real `*bigquery.Client`.
    3.  A global, thread-safe schema registry and a public `RegisterSchema` function.
    4.  The `BigQueryManager` struct and all its methods.
* **(L2-2.5) Internal Validation:** The manager must have its own `Validate` method that `CreateResources` calls first.
* **(L2-1.3) Idempotency:** Use the `handle.Metadata()` method and check for a "not found" error to determine if a resource exists.
* **Schema Inference & Translation:** When creating a table, you **must** look up the schema from the registry, use `bigquery.InferSchema`, and correctly translate partitioning/clustering fields from our generic config into the `bigquery.TableMetadata` struct.
* **(L1-2.1) Efficient Teardown:** The `Teardown` method **must** operate by iterating over datasets only and calling `dataset.DeleteWithContents()`.
* **(L2-1.4) Concurrency:** Use a `sync.WaitGroup` and dedicated channels for errors and provisioned resources.

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `servicemanager/bigquery_test.go`
2.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This revision updates the implementation instructions to match the production code's structure and robustness.

#### Clean Architectural Refinements
* **Idempotency Logic:** Specified the exact `handle.Metadata()` / `handle.Attrs()` -> `errors.Is()` workflow.
    * **Reason:** Enforces a more efficient and robust pattern for checking resource existence, better fulfilling **L2-1.3 (Idempotency)**.
* **Constructor Validation:** Required manager constructors to validate their dependencies.
    * **Reason:** Prevents runtime panics from `nil` dependencies.
* **Schema Translation Logic (BigQuery):** Added an explicit requirement to handle the translation from our generic config to the provider-specific `bigquery.TableMetadata`.
    * **Reason:** This critical logic is the core of the manager's functionality for tables.
* **Internal Validation (BigQuery):** The responsibility for validating the schema registry was moved into the manager's own `Validate` method.
    * **Reason:** This is a better separation of concerns (**L2-1.1**), as the schema registry is a feature of the manager, not the underlying provider client.
* **Efficient Teardown (BigQuery):** Mandated that teardown should be handled at the dataset level.
    * **Reason:** This is significantly more efficient and correctly models the resource hierarchy, better fulfilling **L1-2.1 (Infrastructure Provisioning)**.
* **Combined File Structure (BigQuery):** The prompt now requires the manager, interfaces, and adapter to be in a single file.
    * **Reason:** This reflects the pragmatic choice made in the production code, where the thin BigQuery adapter doesn't warrant a separate file.

#### Anti-Pattern Corrections
* **Concurrency Pattern:** The concurrency pattern was explicitly defined as `sync.WaitGroup` with dedicated channels.
    * **Reason (Anti-Pattern Correction):** Enforces a consistent, robust pattern for **L2-1.4 (Concurrency)** across all managers that supports partial success reporting.