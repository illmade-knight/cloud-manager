# **Prompt 01: Implementation for ConfigGenerator**

**Objective:** Generate the implementation for the `ConfigGenerator` and its helper functions. The generated code must satisfy the previously defined unit tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/configgenerator.go` file.

I have provided a complete unit test file (`configgenerator_test.go`). Your implementation must make all the tests pass by correctly filtering the master `MicroserviceArchitecture` into service-specific resource slices.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust and correct data transformation logic.

**Your Specific Task:** Generate the complete code for `orchestration/configgenerator.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement.
* **Filtering Logic (`buildServiceResourceSpec`):** You must implement the core filtering logic. This function will take a single service and the full architecture as input. It must iterate through all resources in the architecture and build a new `CloudResourcesSpec` containing only the resources that the input service is linked to via the `ProducerService`, `ConsumerService`, `producers`, or `consumers` fields.
* **(L1-2.2) Dependency Inference:** The filtering logic must correctly infer transitive dependencies. Specifically, if a service is linked to a `BigQueryTable`, the implementation must find the corresponding `BigQueryDataset` in the master architecture and also include it in the service-specific configuration.
* **File I/O Helpers:** Implement the `WriteServiceConfigFiles`, `GenerateAndWriteServiceSpecificConfigs`, and `CleanStaleConfigs` functions. These will handle the practical tasks of marshalling the generated configs to YAML and writing/deleting them from the filesystem.

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `orchestration/configgenerator_test.go`
2.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `ConfigGenerator` implementation. It requires the implementation of the core filtering logic, including the inference of transitive resource dependencies.