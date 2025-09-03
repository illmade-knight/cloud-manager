# **Prompt 06: Unit Tests for the Conductor**

**Objective:** Generate a complete unit test file for the `Conductor`. This test will validate the master orchestration logic, ensuring it calls all sub-orchestrators and clients in the correct sequence.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `orchestration/conductor_test.go` file.

Write a comprehensive unit test for a `Conductor` that does not yet exist. The test **must** use mocks for all of the `Conductor`'s dependencies (`IAMOrchestrator`, `DeploymentManager`, etc.). The primary goal is to verify that the `Conductor.Run()` method executes all the deployment phases in the correct, predefined order.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough unit tests for complex, stateful orchestrators using the `testify/mock` library.

**Your Specific Task:** Generate the complete code for `orchestration/conductor_test.go`.

**Key Requirements to Fulfill:**

* **Package:** The test file must be in the `orchestration_test` package.
* **Use Mocks:** You **must** create mock implementations for all of the `Conductor`'s direct dependencies, including `IAMOrchestrator`, `DeploymentManager`, `RemoteDirectorClient`, and `prerequisites.Manager`.
* **Test `Run()` Method Sequencing:** The main test case must validate the entire successful workflow. You must use `testify/mock`'s features to set up a chain of expected calls and assert that they happen in the correct order. The expected sequence is:
    1.  `mockPrereqManager.On("CheckAndEnable", ...)`
    2.  `mockIAMOrchestrator.On("SetupServiceDirectorIAM", ...)`
    3.  `mockIAMOrchestrator.On("VerifyServiceDirectorIAM", ...)`
    4.  *(and so on for every phase in the correct sequence)*
* **Test Options Logic:** Write a sub-test to validate the `ConductorOptions`. For example, create the `Conductor` with `SetupIAM: false`, run the test, and assert that the methods on the `mockIAMOrchestrator` were **not** called, while others were.

**Dependencies (Full source code to be provided):**

1.  All interface files for the `Conductor`'s dependencies.
2.  `servicemanager/systemarchitecture.go`.

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `Conductor` unit tests. Its primary focus is to validate the correctness of the high-level, multi-phase orchestration sequence.