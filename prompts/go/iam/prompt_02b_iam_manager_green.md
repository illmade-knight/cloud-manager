# **Prompt 02: Implementation for IAMManager**

**Objective:** Generate the complete implementation for the `IAMManager`. The generated code must satisfy the previously defined unit tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/iammanager.go` file.

I have provided a complete unit test file (`iammanager_test.go`). Your implementation of the `IAMManager` **must** make all the tests in that file pass by correctly orchestrating its `RolePlanner` and `IAMClient` dependencies.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in cloud security and Infrastructure as Code.

**Your Specific Task:** Generate the complete code for `iam/iammanager.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement.
* **Constructor:** Implement a `NewIAMManager` constructor that takes an `IAMClient` and `RolePlanner` as dependencies.
* **Orchestration Logic (`ApplyIAMForDataflow`):** The method must perform the following steps in order:
    1.  Iterate through all services in the specified dataflow and call `client.EnsureServiceAccountExists` for each, storing the returned email addresses in a map.
    2.  Call `planner.PlanRolesForApplicationServices` to get the flat list of required `IAMBinding`s.
    3.  **(L2-2.2) Implement Policy Assembly:** Create and execute the logic to transform the flat `IAMBinding` slice into a `map[resourceKey]PolicyBinding`. This involves iterating through the plan and grouping all roles and members for each unique resource. This is the core logic of the manager.
    4.  Iterate through the assembled map of policies and make a single, atomic `client.ApplyIAMPolicy` call for each resource.
* **(L2-1.1) Separation of Concerns:** The `IAMManager` must only contain orchestration logic. It should not contain any logic for *how* to determine roles (that is the planner's job) or *how* to apply policies to a specific cloud provider (that is the client's job).

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `iam/iammanager_test.go`
2.  **The Contracts:** `iam/iamplanner.go` and `iam/iamadapter.go`
3.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `IAMManager` implementation.