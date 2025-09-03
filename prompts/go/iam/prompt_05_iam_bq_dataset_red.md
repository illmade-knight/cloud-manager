# **Prompt 05: Tests for BigQueryIAMManager (Admin Tool)**

**Objective:** Generate a complete test suite for the `BigQueryIAMManager`. This includes a unit test for its internal role-translation logic and an integration test for its live API interactions.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate two test files in the `iam_test` package:
1.  **`google_dataset_iam_test.go`**: A **unit test** to validate the internal logic that translates modern IAM roles to legacy BigQuery ACL roles.
2.  **`google_dataset_iam_integration_test.go`**: An **integration test** that uses a live GCP environment to validate the full Add -> Check -> Remove lifecycle of a dataset-level IAM binding.

## **Reinforcement Prompt (Detailed Context)**

### **File 1: `google_dataset_iam_test.go` (Unit Tests)**
* **Purpose:** This file tests the manager's internal logic in isolation.
* **Test Coverage:**
    * **(L2-1.2) Role Translation:** Write a table-driven unit test for the `translateToLegacyRole` helper function. It must verify that modern roles (e.g., `roles/bigquery.dataViewer`) are correctly converted to their legacy `bigquery.AccessRole` equivalents (e.g., `bigquery.ReaderRole`).

### **File 2: `google_dataset_iam_integration_test.go` (Integration Tests)**
* **Purpose:** This file tests the manager's end-to-end functionality against the live BigQuery API.
* **Tagging:** Must include the `//go:build integration` build tag.
* **Test Coverage:**
    * **(L1-2.2) Full IAM Lifecycle:** Write a test that performs the full Add -> Check -> Remove lifecycle.
    * **Prerequisites:** The test must programmatically create a temporary BigQuery dataset to apply policies to and ensure it is cleaned up.
    * **(L2-2.3) Eventual Consistency & Translation:** The test **must** use a polling mechanism (`require.Eventually`) when checking for the binding. The `CheckDatasetIAMBinding` call will use a modern role, and the test must succeed, thus implicitly validating that the manager's internal logic correctly checks for the translated legacy role.

---
## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `BigQueryIAMManager`. It is designed to generate the component as a standalone administrative tool, not as part of the core automated IAM manager.