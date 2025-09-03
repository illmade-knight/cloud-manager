# **Prompt 01: Deployment Package Interfaces**

**Objective:** Generate all the foundational Go interface files for the `deployment` package. These files define the contracts for the main deployer and its internal dependencies, enabling a clean, testable architecture.

**TDD Phase:** Foundation

## **Primary Prompt (Concise)**

Your task is to generate the Go code for two foundational interface files in the `deployment` package:
1.  **`deployment/containerdeployer.go`**: Contains the primary, public-facing `ContainerDeployer` interface for the package.
2.  **`deployment/internal_adapters.go`**: Contains the unexported, internal interfaces that the main orchestrator will depend on (e.g., `storageAPI`, `cloudBuildAPI`).

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer and system architect specializing in API design and defining clean, decoupled contracts.

**Your Specific Task:** Generate the complete code for the two interface files described below.

### **File 1: `deployment/containerdeployer.go`**
This file defines the main public contract for the entire deployment package.

* **Package:** `deployment`
* **Interface `ContainerDeployer`:**
    * `Build(ctx context.Context, serviceName string, spec servicemanager.DeploymentSpec) (string, error)`: This method handles all steps required to produce a container image (e.g., uploading source, triggering a build) and must return the final, complete image URI.
    * `DeployService(ctx context.Context, serviceName, serviceAccountEmail string, spec servicemanager.DeploymentSpec) (string, error)`: This method deploys a pre-built image from the `spec.Image` field to the target platform and must return the final service URL.
    * `BuildAndDeploy(ctx context.Context, serviceName, serviceAccountEmail string, spec servicemanager.DeploymentSpec) (string, error)`: Orchestrates the full, sequential `Build` and `DeployService` workflow.
    * `Teardown(ctx context.Context, serviceName string) error`: Deletes the deployed service from the target platform.

### **File 2: `deployment/internal_adapters.go`**
This file defines the unexported interfaces that the `CloudBuildDeployer` will use to interact with specific cloud services. This abstraction is critical for enabling unit testing of the orchestrator.

* **Package:** `deployment`
* **Interfaces to define:**
    * **`storageAPI`:**
        * `EnsureBucketExists(ctx context.Context, bucketName string) error`
        * `Upload(ctx context.Context, sourceDir, bucket, objectName string) error`
    * **`artifactRegistryAPI`:**
        * `EnsureRepositoryExists(ctx context.Context, repoName, region string) error`
    * **`cloudBuildAPI`:**
        * `TriggerBuildAndWait(ctx context.Context, gcsSourceObject string, spec servicemanager.DeploymentSpec) error`
    * **`cloudRunAPI`:**
        * `CreateOrUpdate(ctx context.Context, serviceName, saEmail string, spec servicemanager.DeploymentSpec) (*run.GoogleCloudRunV2Service, error)`
        * `Delete(ctx context.Context, serviceName string) error`

**Dependencies (Full source code to be provided):**

1.  `servicemanager/systemarchitecture.go` (contains the `DeploymentSpec` struct)

---

## Prompt Changelog

### RV_20250901

This is the initial version of the foundational interfaces prompt for the `deployment` package. It establishes the public contract (`ContainerDeployer`) and the internal dependency contracts (`storageAPI`, etc.) required for a clean, testable orchestrator design.