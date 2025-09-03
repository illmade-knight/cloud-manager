# **Prompt 06: Implementation for the Conductor**

**Objective:** Generate the implementation for the master `Conductor`. The generated code must satisfy the previously defined unit tests by correctly sequencing and coordinating all other modules.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/conductor.go` file.

I have provided a complete unit test file (`conductor_test.go`). Your implementation of the `Conductor` **must** make all the tests pass by implementing the multi-phase `Run()` method, calling all sub-orchestrators in the correct order.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer and system architect, specializing in designing complex, distributed workflows and CI/CD automation systems.

**Your Specific Task:** Generate the complete code for `orchestration/conductor.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **State Management:** The `Conductor` struct must be implemented to hold instances of all its sub-orchestrator dependencies and to manage the state that is passed between phases (e.g., `serviceAccountEmails`, `builtImageURIs`, `directorURL`).
* **(L1-1.2) `Run()` Method Implementation:** The `Run` method is the core of the implementation. It **must** be implemented as a sequence of private helper methods, one for each phase, that are called in the exact required order:
    1.  `checkPrerequisites(...)`
    2.  `prepareBuildArtifactsPhase(...)`
    3.  `preflightServiceConfigsPhase(...)`
    4.  `setupIAMPhase(...)`
    5.  `buildAndDeployPhase(...)`
    6.  `triggerRemoteFoundationalSetup(...)`
    7.  `verifyResourceLevelIAMPhase(...)`
    8.  `deploymentPhase(...)`
    9.  `triggerRemoteDependentSetup(...)`
* **Options Handling:** The implementation of each phase-based helper method (e.g., `setupIAMPhase`) **must** be wrapped in an `if c.options.SomePhase { ... }` block to allow the caller to skip phases, as required by the unit test.
* **(L2-1.1) Pure Orchestration:** The `Conductor` must contain no low-level logic. Its methods should only call the methods of its direct dependencies (`IAMOrchestrator`, `DeploymentManager`, etc.).

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `orchestration/conductor_test.go`
2.  **The Contracts:** All interface files for the `Conductor`'s dependencies.
3.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `Conductor` implementation. It requires a stateful, multi-phase orchestrator that correctly sequences all other modules in the system.