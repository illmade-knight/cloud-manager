# **Prompt 03: Implementation for Cloud Run Adapter**

**Objective:** Generate the implementation for the `cloudrunadapter.go` file. The code must satisfy the previously defined unit and integration tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `deployment/cloudrunadapter.go` file.

Your implementation must make all tests in `cloudrunadapter_test.go` and `cloudrunadapter_integration_test.go` pass. It must correctly handle the create-or-update logic and poll for the completion of long-running deployment operations.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `deployment/cloudrunadapter.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **(L2-1.3) Idempotency Logic:** The `CreateOrUpdateService` method **must** first call the `run.Service.Projects.Locations.Services.Get` method. Based on the error returned (checking for `codes.NotFound`), it must then call either the `Create` or `Patch` method accordingly.
* **(L2-1.2) Spec Translation:** The implementation must contain the logic to build a `*run.GoogleCloudRunV2Service` struct from our generic `DeploymentSpec`, correctly mapping all specified fields:
    * `Image`
    * `ServiceAccount`
    * `EnvironmentVars`
    * `SecretEnvironmentVars` (translating to `SecretKeyRef`)
    * `CPU` and `Memory` resource limits
    * `MinInstances` and `MaxInstances` scaling parameters
* **(L2-2.3) Handle Long-Running Operations:** After calling `Create` or `Patch`, the implementation **must** obtain the long-running operation name and enter a polling loop, waiting for the operation to complete successfully before returning.

**Dependencies:**

1.  **The Specifications:** `deployment/cloudrunadapter_test.go` and `deployment/cloudrunadapter_integration_test.go`
2.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the Cloud Run adapter implementation. It requires a robust implementation that handles idempotency, detailed configuration translation, and polling for long-running operations.