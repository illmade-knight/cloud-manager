# **Prompt 02: Implementation for Artifact Registry Helper**

**Objective:** Generate the implementation for the `EnsureArtifactRegistryRepositoryExists` helper function. The generated code must satisfy the previously defined integration test.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `deployment/artifacts.go` file.

I have provided an integration test in `artifacts_integration_test.go`. Your implementation of `EnsureArtifactRegistryRepositoryExists` must make the tests pass by correctly checking for a repository's existence and creating it if it's not found.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust, production-grade clients for Google Cloud Platform.

**Your Specific Task:** Generate the complete code for `deployment/artifacts.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **(L2-1.3) Idempotency Logic:** The implementation **must** be idempotent. It must first call the Artifact Registry client's `GetRepository` method.
    * If the call succeeds (returns no error), the function should log that the repository already exists and return `nil`.
    * If the call returns a `codes.NotFound` error, the function should proceed to create the repository.
    * If the call returns any other error, it should be propagated to the caller.
* **Creation Logic:** When creating the repository, it must be configured with the `DOCKER` format.
* **(L2-2.3) Handle Long-Running Operations:** The `CreateRepository` call returns a long-running operation. The implementation **must** call `.Wait(ctx)` on the operation to ensure the repository is fully created before the function returns.

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `deployment/artifacts_integration_test.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the Artifact Registry helper implementation. It requires the function to be idempotent and to correctly handle long-running API operations.