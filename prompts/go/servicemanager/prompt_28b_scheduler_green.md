# **Prompt 28: Implementation for Scheduler Adapter (RV_20250901_CORR4)**

**Objective:** Generate the complete implementation for the `googlescheduler.go` adapter.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the full code for `servicemanager/googlescheduler.go`. The implementation must make all tests in `googlescheduler_test.go` and `googlescheduler_integration_test.go` pass. It must include a test-only constructor for unit testing.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete, full-file code for `servicemanager/googlescheduler.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **(L2-1.2) Full Interface Fulfillment.**
* **(L2-2.5) Validation Logic:** Implement the `Validate` method using a robust cron parsing library.
* **(L1-2.1) Request Building:** Correctly translate the generic config into the `schedulerpb.Job` protobuf.
* **(L2-1.3) Idempotency:** The `Teardown` logic should correctly handle "not found" errors.
* **Test-Only Constructor:** You **must** include an exported, test-only constructor (e.g., `NewTestGcpSchedulerClientAdapter`) that allows the test suite to instantiate the adapter struct with a `nil` or mock client.

---

## Prompt Changelog

### RV_20250901_CORR4

This revision adds a correction to ensure proper test isolation for unit tests.

#### Anti-Pattern Corrections
* **Unit Test Isolation:** The prompt now requires a test-only constructor in the implementation file.
    * **Reason (Anti-Pattern Correction):** A previous test generation produced a unit test that called the production factory (`Create...Client`), which attempted a real network connection and caused a panic. Unit tests **must not** instantiate live clients. Requiring a test-only constructor allows the test suite to instantiate the adapter with a `nil` client to test internal logic (like `Validate`) in complete isolation.