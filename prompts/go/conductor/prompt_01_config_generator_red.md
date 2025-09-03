# **Prompt 01: Unit Tests for ConfigGenerator**

**Objective:** Generate a complete unit test file for the `ConfigGenerator`. This test will validate the logic of filtering a master architecture configuration into multiple, service-specific resource configurations.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/configgenerator_test.go` file.

Write a comprehensive unit test for the `GenerateServiceConfigs` function, which does not yet exist. The test must create a sample `MicroserviceArchitecture` and assert that the function correctly filters the resources for each service based on `ProducerService`, `ConsumerService`, and `producers`/`consumers` links.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough, table-driven unit tests for complex data transformation logic.

**Your Specific Task:** Generate the complete code for `orchestration/configgenerator_test.go`.

**Key Requirements to Fulfill:**

* **Package:** The test file must be in the `orchestration_test` package.
* **No Mocks:** The `ConfigGenerator` is pure logic, so this test **must not** use any mocks.
* **Comprehensive Test Architecture:** The test setup must create a detailed `MicroserviceArchitecture` struct with multiple services and resources. This architecture should include:
    * A topic linked to a producer service.
    * A subscription linked to a consumer service.
    * A GCS bucket linked to a producer.
    * A BigQuery table linked to a consumer, which in turn depends on a dataset.
    * Resources that are *not* linked to any service.
* **Test `GenerateServiceConfigs`:** The main test function should call `GenerateServiceConfigs` and then perform detailed assertions on the returned slice of `ServiceConfig`.
* **Helper Function:** Include a test-local helper function (e.g., `findConfigForService`) to easily find the generated config for a specific service within the result slice to make assertions cleaner.
* **Assertions:** The test must validate the filtering logic precisely:
    * For a "publisher" service, assert that its generated config contains the topic it produces to, but **not** other unrelated resources.
    * For a "consumer" service, assert that its config contains the resource it consumes from.
    * **(L1-2.2) Dependency Inference:** For a service linked to a BigQuery table, assert that its config contains both the table **and** its parent dataset.

**Dependencies (Full source code to be provided):**

1.  `orchestration/configgenerator.go` (will contain the `ServiceConfig` struct)
2.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `ConfigGenerator` unit tests. It focuses on validating the core business logic of filtering and resource dependency inference.