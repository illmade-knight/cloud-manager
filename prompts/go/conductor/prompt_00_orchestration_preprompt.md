# **Meta-Prompt: Orchestration Package Generation**

**Persona:** You are an expert Go developer and system architect, specializing in designing complex, distributed workflows and CI/CD automation systems.

---
## **1. Package Overview**

We are about to generate the complete Go code for the **`orchestration` package**. This is the top-level "brain" of the entire system. It is responsible for sequencing and coordinating all other modules (`servicemanager`, `iam`, `deployment`) into a coherent, multi-phase workflow.

The architecture is composed of several distinct, testable components:
1.  **Utilities (`configgenerator.go`, `preflightvalidator.go`):** These are build-time tools. The `ConfigGenerator` creates service-specific `resources.yaml` files. The `PreflightValidator` runs local tests on each service to validate these generated files *before* any cloud resources are touched.
2.  **Sub-Orchestrators (`IAMOrchestrator`, `DeploymentManager`):** These components handle the complex, often parallel, logic for their specific domains (e.g., creating all SAs and verifying IAM propagation; building all container images in parallel).
3.  **`RemoteDirectorClient`:** This component manages the event-driven communication with a deployed `ServiceDirector` instance over Pub/Sub.
4.  **`Conductor`:** The master orchestrator that holds instances of the sub-orchestrators and executes the main `Run()` workflow in the correct, sequential order.

---
## **2. Methodology & Guidelines**

We will follow our established "Red -> Green -> Refine" TDD workflow for **each component**. All previously established rules apply: standardized mocking, correct package naming, full file generation, additive changelogs, and requirements traceability.

---
## **3. Test Data Generation (`testdata/`)**

A separate set of prompts will be used to generate the sample application services in the `testdata` directory. These are the actual Go applications that our orchestrator will deploy.

* **Purpose:** The goal is to generate simple, runnable Go services (a publisher, a subscriber, a director).
* **Pre-flight Tests:** The "Red" prompt for each `testdata` service will generate a **pre-flight test**. This is a simple `_test.go` file that simulates being run by the `PreflightValidator`. Its job is to load the `resources.yaml` file embedded in its own binary and verify that the expected resource names are present, confirming the `ConfigGenerator` worked correctly.