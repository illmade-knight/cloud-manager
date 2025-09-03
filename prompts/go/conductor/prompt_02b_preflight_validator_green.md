# **Prompt 02: Implementation for PreflightValidator**

**Objective:** Generate the implementation for the `PreflightValidator`. The generated code must satisfy the previously defined unit tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/preflightvalidator.go` file.

I have provided a complete unit test file (`preflightvalidator_test.go`). Your implementation must make the tests pass by correctly inspecting a `MicroserviceArchitecture` and generating a slice of environment variable strings for a given service.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing automation and build tooling.

**Your Specific Task:** Generate the complete code for `orchestration/preflightvalidator.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **`GenerateTestEnvForService` Logic:** Implement the core logic that takes a service name and the full architecture as input. It must iterate through all resources in all dataflows and check the `ProducerService`, `ConsumerService`, `producers`, and `consumers` fields. When a link to the target service is found, it must generate the appropriate `EXPECTED_...` environment variable string and add it to the result slice.
* **Special Case for ServiceDirector:** The logic must include special handling for the `ServiceManagerSpec`. When the target service is the ServiceDirector, it must generate the specific environment variables for its unique resources (e.g., `EXPECTED_COMMAND_TOPIC`).
* **`Run` Method Implementation:** Implement the `Run` method which:
    1.  Iterates through all services in the architecture.
    2.  For each service, calls `GenerateTestEnvForService` to get its test environment.
    3.  Uses the `os/exec` package to execute the `go test ./...` command within the service's buildable module directory, injecting the generated environment variables.
    4.  It must capture the output and return an error if any of the test commands fail.

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `orchestration/preflightvalidator_test.go`
2.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `PreflightValidator` implementation. It requires the implementation of the core logic for generating test environments and the executor logic for running the local pre-flight tests.