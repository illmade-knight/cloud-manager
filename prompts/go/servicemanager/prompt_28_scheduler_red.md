# **Prompt 28: Unit & Integration Tests for Scheduler Adapter (RV_20250901_CORR4)**

**Objective:** Generate the complete test suite for the `googlescheduler.go` adapter.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate two test files in the `servicemanager_test` package:
1.  **`googlescheduler_test.go`**: A **unit test** using `testify/mock` to validate the adapter's internal logic.
2.  **`googlescheduler_integration_test.go`**: An **integration test** that uses a live GCP environment to validate the full resource lifecycle.

## **Reinforcement Prompt (Detailed Context)**

### **File 1: `googlescheduler_test.go` (Unit Tests)**
* **Package:** Must be `servicemanager_test`.
* **Mocking:** Use `testify/mock`.
* **Test Coverage:**
    * **(L2-2.5) `Validate` Method:** A test suite for the `Validate` method.
    * **(L1-2.1) Request Building:** A test for the `CreateJob` method.

### **File 2: `googlescheduler_integration_test.go` (Integration Tests)**
* **Package:** Must be `servicemanager_test`.
* **Environment:** Runs against a live GCP API.
* **Prerequisites:** Must use the modern `pubsub/v2` client for setting up a prerequisite topic.
* **Tagging:** Must include the `//go:build integration` build tag.
* **Test Coverage:**
    * **(L1-2.1) Full Lifecycle:** Write a test that:
        1. Reads `GCP_PROJECT_ID`, `GCP_REGION`, and `GCP_TEST_SERVICE_ACCOUNT` from environment variables, skipping if they are not set.
        2. Creates a prerequisite Pub/Sub topic.
        3. Creates a valid `CloudSchedulerJob` struct, **populating the `ServiceAccount` field** with the value from the environment variable.
        4. Creates the scheduler job targeting the topic.
        5. Verifies the job exists, then deletes it and verifies deletion.
        6. Cleans up the Pub/Sub topic.

---

## Prompt Changelog

### RV_20250901_CORR4

This revision adds corrections to ensure test setups are complete and valid for live API interaction.

#### Anti-Pattern Corrections
* **Incomplete Test Setup:** The prompt now requires the integration test to populate all necessary fields for a valid API request, specifically the `ServiceAccount`.
    * **Reason (Anti-Pattern Correction):** A previous generation created a test that failed with an `InvalidArgument` error because it sent an incomplete request to the live API. Integration tests **must** be configured with valid data, reading sensitive or environment-specific values (like service accounts) from environment variables.
* **Enforced Modern SDK for Test Prerequisites:** The prompt requires the use of the `pubsub/v2` client for setting up resources in integration tests to maintain consistency.
* **Unit Test Isolation:** The prompts require a test-only constructor for the Green phase to ensure unit tests do not make live network calls.