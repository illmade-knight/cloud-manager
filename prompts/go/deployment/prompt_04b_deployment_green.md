# **Prompt 04: Implementation for CloudBuildDeployer**

**Objective:** Generate the implementation for the `CloudBuildDeployer` and its internal adapters. The code must satisfy all previously defined unit and integration tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `deployment/cloudbuilddeployer.go` file.

Your implementation must make all tests in `cloudbuilddeployer_test.go` and `cloudbuilddeployer_integration_test.go` pass. The file must contain the `CloudBuildDeployer` orchestrator and the co-located, unexported adapter implementations for GCS, Artifact Registry, Cloud Build, and Cloud Run.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `deployment/cloudbuilddeployer.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **(L2-1.1) Orchestrator Pattern:** The `CloudBuildDeployer` struct must act as an orchestrator, depending on the internal adapter interfaces (`storageAPI`, etc.), not on any concrete Google Cloud SDK clients.
* **Co-located Adapters:** You **must** implement the unexported adapter structs (e.g., `googleStorageAdapter`, `googleCloudBuildAdapter`, etc.) within the same `cloudbuilddeployer.go` file. These adapters will contain the direct calls to the Google Cloud SDKs.
* **Orchestration Logic:** The `BuildAndDeploy` method must execute the full pipeline by calling the internal adapter interfaces in the correct sequence. You should also implement the separate `Build` and `DeployService` methods to allow for a two-phase workflow.
* **Cloud Build Configuration:** The `googleCloudBuildAdapter` **must** configure the Cloud Build job to use the `gcr.io/k8s-skaffold/pack` builder. It must also contain the critical logic to handle monorepo builds by prepending a build step to copy `go.mod` and `go.sum` files into the service's subdirectory.
* **Source Packaging:** The `googleStorageAdapter.Upload` method must correctly package the source code directory into a `.tar.gz` archive before uploading.
* **(L2-2.3) Handle Long-Running Operations:** The adapters for Cloud Build and Cloud Run **must** correctly poll and wait for their respective long-running operations to complete before returning.

**Dependencies:**

1.  **The Specifications:** `cloudbuilddeployer_test.go`, `cloudbuilddeployer_integration_test.go`
2.  **The Contracts:** `deployment/containerdeployer.go`, `deployment/internal_adapters.go`
3.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `CloudBuildDeployer` implementation. It requires a robust orchestrator that delegates to co-located, internal adapters and correctly handles complex build configurations and long-running cloud operations.