# **Meta-Prompt: Deployment Package Generation**

**Persona:** You are an expert Go developer and system architect with deep, practical experience in designing and building CI/CD and cloud-native deployment automation pipelines on Google Cloud Platform.

---
## **1. Package Overview**

We are about to generate the complete Go code for the **`deployment` package**. This package is the engine that turns a service's source code into a live, running application. Its primary component, the `CloudBuildDeployer`, orchestrates a complex, multi-stage pipeline across several distinct Google Cloud services.

The orchestrated workflow is as follows:
1.  **Prepare Prerequisites:** Ensure a Google Cloud Storage (GCS) bucket for source code and a Google Artifact Registry repository for container images exist, creating them if necessary.
2.  **Upload Source:** Package the service's source code into a compressed archive and upload it to the GCS bucket.
3.  **Build Container Image:** Trigger a Google Cloud Build job that uses Cloud Native Buildpacks (`pack`) to transform the source code into a runnable container image, pushing the final image to Artifact Registry.
4.  **Deploy Service:** Deploy the newly built container image to Google Cloud Run, creating or updating the service.

---
## **2. Methodology & Guidelines**

We will follow our established "Red -> Green -> Refine" TDD workflow and adhere to all previously established rules (standardized mocking, `_test` packages, full file generation, etc.).

For this package, we will use our **Hybrid Testing Strategy**:
* A comprehensive **unit test** will validate the `CloudBuildDeployer`'s orchestration logic in isolation, using mocks for the underlying cloud service APIs.
* A robust **integration test** will validate the entire "source-to-URL" pipeline from end to end by interacting with live GCP services.

---
## **3. Key Architectural Patterns to Enforce**

* **Orchestrator with Internal Adapters:** The `CloudBuildDeployer` must be implemented as an orchestrator. It **must not** contain direct SDK calls. Instead, it will depend on a set of **internal, unexported interfaces** (e.g., `storageAPI`, `cloudBuildAPI`).
* **Co-located Implementations:** The concrete implementations of these internal interfaces (the "adapters," e.g., `googleStorageAdapter`) should be implemented as unexported structs within the **same `cloudbuilddeployer.go` file**. This pattern keeps the implementation details tightly encapsulated with the orchestrator that uses them.
* **Handling Long-Running Operations:** The implementations for Cloud Build and Cloud Run **must** correctly handle long-running operations. They should trigger an operation and then poll its status, waiting for it to complete successfully before allowing the orchestrator to proceed to the next step.