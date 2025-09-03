# **Prompt 02: Unit Tests for PreflightValidator**

**Objective:** Generate a complete unit test file for the `PreflightValidator`. This test will validate the logic of generating service-specific test environment variables from a master architecture configuration.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/preflightvalidator_test.go` file.

Write a comprehensive unit test for the `PreflightValidator`'s `GenerateTestEnvForService` method, which does not yet exist. The test must create a sample `MicroserviceArchitecture` and assert that the function correctly generates the expected environment variables for several different services based on their resource links.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough, table-driven unit tests for complex data transformation logic.

**Your Specific Task:** Generate the complete code for `orchestration/preflightvalidator_test.go`.

**Key Requirements to Fulfill:**

* **Package:** The test file must be in the `orchestration_test` package.
* **No Mocks:** The `PreflightValidator`'s core logic is pure; this test **must not** use any mocks.
* **Comprehensive Test Architecture:** The test setup must create a detailed `MicroserviceArchitecture` struct with services linked to various resource types (topics, GCS buckets, etc.). It must also include the `ServiceManagerSpec` (the ServiceDirector) to test its special case.
* **Table-Driven Test:** The test must be table-driven, with each case testing the output of `GenerateTestEnvForService` for a different service in the architecture.
* **Assertions:** For each test case, assert that the returned slice of environment variable strings (`[]string`) contains the expected values (e.g., `["EXPECTED_TOPIC_NAME=topic-a"]`). You **must** use `assert.ElementsMatch` because the order of the generated environment variables is not guaranteed.

**Dependencies (Full source code to be provided):**

1.  `orchestration/preflightvalidator.go`
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `PreflightValidator` unit tests. It focuses on validating the core business logic of inspecting the architecture and generating the correct test environment for each service.