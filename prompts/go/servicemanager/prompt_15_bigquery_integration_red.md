# **Prompt 15: Integration Test for BigQueryManager (RV_20250901)**

**Objective:** Generate a complete **integration test** file (`bigquery_integration_test.go`) to validate the `BigQueryManager`'s full lifecycle against an emulator.

**TDD Phase:** Red (Generate the test that the full implementation must pass).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/bigquery_integration_test.go` file.

Write a complete integration test for the **`BigQueryManager`** (not the adapter). The test must use a BigQuery emulator to validate the full Create, Verify, and Teardown lifecycle.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust integration tests.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/bigquery_integration_test.go`**.

**Key Requirements to Fulfill:**
* **Test Target:** The test **must** instantiate and test the `BigQueryManager`, not just the adapter.
* **Environment:** The test must use an emulator.
* **Setup:**
    * It must register a test schema (e.g., `BQTestSchema`).
    * It must create a real `*bigquery.Client`, wrap it in the `NewAdapter`, and inject it into the `NewBigQueryManager`.
* **Test Coverage:**
    * **(L1-2.1) Full Lifecycle:** Write a test that runs the manager's `CreateResources`, `Verify`, and `Teardown` methods sequentially.
    * **Direct Verification:** After `CreateResources` runs, the test must use the raw `*bigquery.Client` to directly check the emulator and assert that the table was created with the correct metadata (e.g., partitioning, clustering).
    * **Robust Teardown Verification:** The test must include the sophisticated polling logic (`assert.Eventually`) to verify dataset deletion, with a fallback to `t.Skip` in a CI environment to handle known emulator flakiness.
* **Tagging:** The file **must** include the `//go:build integration` build tag.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/bigquery.go` (The manager and adapter implementation)
2.  `servicemanager/systemarchitecture.go`
3.  `emulators/bigquery.go` (The emulator setup helpers)