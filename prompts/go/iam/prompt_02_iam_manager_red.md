# **Prompt 02: Unit Tests for IAMManager**

**Objective:** Generate the complete unit test file for the `IAMManager`. This test will validate the high-level orchestration logic, ensuring it correctly uses its dependencies and groups permissions for atomic application.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/iammanager_test.go` file.

Write a comprehensive unit test for an `IAMManager` that does not yet exist. The test **must** use mocks for the `RolePlanner` and `IAMClient` interfaces. The primary goal is to verify that the manager correctly takes a flat list of permissions from the planner, groups them by resource, and makes a single `ApplyIAMPolicy` call per resource.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough unit tests for complex orchestration logic using the `testify/mock` library.

**Your Specific Task:** Generate the complete code for `iam/iammanager_test.go`.

**Key Requirements to Fulfill:**

* **Use Mocks:** You **must** use `testify/mock` to create mock implementations for the `RolePlanner` and `IAMClient` interfaces.
* **Test `ApplyIAMForDataflow` Logic:** The test must validate the entire orchestration flow:
    1.  **Mock Service Account Creation:** Set up the mock `IAMClient` to expect calls to `EnsureServiceAccountExists` for each service in the test architecture and return mock email addresses.
    2.  **Mock the Planner:** Set up the mock `RolePlanner` to return a predefined, flat slice of `IAMBinding` structs. This slice should include multiple bindings for the same resource to test the grouping logic.
    3.  **Assert Policy Assembly:** The test's most critical assertion is on the call to `mockClient.ApplyIAMPolicy`. You must capture the `PolicyBinding` argument passed to this method. Assert that the flat plan was correctly assembled into a grouped policy.
        * For a resource that had multiple bindings in the flat plan, assert that `ApplyIAMPolicy` was called **only once** for that resource.
        * Assert that the `PolicyBinding` struct passed to the mock contains all the members and roles from the original flat plan, correctly grouped.

**Dependencies (Full source code to be provided):**

1.  `iam/iamplanner.go` (contains the `RolePlanner` interface and `IAMBinding` struct)
2.  `iam/iamadapter.go` (contains the `IAMClient` interface and `PolicyBinding` struct)
3.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `IAMManager` unit tests, created based on the patterns established during the refinement of the `servicemanager` package.