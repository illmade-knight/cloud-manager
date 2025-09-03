# **Prompt 03: Unit Tests for DeploymentManager**

**Objective:** Generate a complete unit test file for the `DeploymentManager`. This test will validate the orchestration logic, with a strong focus on verifying parallel execution.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/deploymentorchestrator_test.go` file.

Write a comprehensive unit test for a `DeploymentManager` that does not yet exist. The test **must** use a mock `ContainerDeployer`. The key requirement is to verify that the `BuildAllServices` and `DeployApplicationServices` methods execute their tasks concurrently, not sequentially.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing robust unit tests for concurrent systems.

**Your Specific Task:** Generate the complete code for `orchestration/deploymentorchestrator_test.go`.

**Key Requirements to Fulfill:**

* **Package:** The test file must be in the `orchestration_test` package.
* **Use Mocks:** You **must** use a hand-rolled mock implementation of the `deployment.ContainerDeployer` interface. This mock is critical for concurrency testing.
    * The mock's `Build` and `DeployService` methods should accept a configurable `time.Sleep` delay.
    * The mock must use a `sync.Mutex` to safely record the number of times its methods are called for each service.
* **(L2-1.4) Test for Parallel Execution:** The tests for `BuildAllServices` and `DeployApplicationServices` must explicitly validate concurrency. The test should:
    1.  Instantiate the mock deployer with a delay (e.g., 100ms).
    2.  Record the start time.
    3.  Call the manager's method to build/deploy multiple services (e.g., 3 services).
    4.  Calculate the total execution duration.
    5.  Assert that the total duration is significantly less than the sequential duration (e.g., less than 250ms, not over 300ms), proving the operations ran in parallel.
* **Test Correctness:**
    * Assert that the mock's methods were called the correct number of times for each service.
    * Assert that errors returned by the mock are correctly aggregated and propagated by the manager.

**Dependencies (Full source code to be provided):**

1.  `orchestration/deploymentorchestrator.go`
2.  `deployment/containerdeployer.go`
3.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `DeploymentManager` unit tests. It has a strong focus on validating the component's primary responsibility: concurrent execution of build and deploy tasks.