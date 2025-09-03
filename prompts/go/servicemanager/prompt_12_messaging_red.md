# **Prompt 12: Unit Tests for MessagingManager (RV_20250901)**

**Objective:** Generate the complete unit test file for the MessagingManager. These tests will serve as the executable specification for the MessagingManager's implementation.

**TDD Phase:** Red (Generate failing tests before the code exists).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/messagingmanager_test.go` file.

Using the provided `MessagingClient` interface, write a comprehensive suite of unit tests for a `MessagingManager` struct that does not yet exist. The tests **must** use mocks for the `MessagingClient` and its handle interfaces (`MessagingTopic`, `MessagingSubscription`) to validate all logic in isolation.

**Key Requirements to Test:**
* **(L2-2.3) Validation:** The tests must verify that the manager calls the client's `Validate` method before creating any resources.
* **(L1-2.1) Dependency Order:** The tests must ensure topics are processed before subscriptions in `CreateResources`, and that subscriptions are deleted before topics in `Teardown`.
* **(L2-1.3) Idempotency:** `CreateResources` tests must cover creating new resources and updating existing ones. `Teardown` tests must show that "not found" errors are ignored.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough unit tests using the testify suite (mock, require, assert).

**Overall Goal:** We are building the `servicemanager` package using a TDD approach. Our next step is to write the tests that will define and validate the behavior of the `MessagingManager`.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/messagingmanager_test.go`**. This file will contain a full suite of unit tests for the `MessagingManager`.

**Key Requirements to Fulfill:**
* **Use Mocks:** You **must** use `testify/mock` to create mocks for `MessagingClient`, `MessagingTopic`, and `MessagingSubscription`.
* **Test Coverage:** You **must** create test cases for the following scenarios:
    * **`CreateResources`**:
        * A failure case where `client.Validate()` returns an error, ensuring no create/update operations are attempted.
        * Success case where new topics and subscriptions are created. Existence should be checked via `topic.Exists()` and `subscription.Exists()`.
        * Success case where topics and subscriptions already exist, ensuring `subscription.Update()` is called.
        * A failure case where the parent topic for a subscription does not exist, ensuring the subscription is not created.
    * **`Teardown`**:
        * Success case where subscriptions are deleted, followed by topics.
        * A case where a resource's `Delete()` method returns a "not found" error, ensuring the overall `Teardown` operation does **not** fail.
        * A case where `TeardownProtection` is enabled on resources, ensuring they are not deleted.

**Dependencies (Full source code to be provided):**

1.  `servicemanager/messagingadapter.go` (contains the interfaces to be mocked)
2.  `servicemanager/systemarchitecture.go` (contains the config structs)

---

## Prompt Changelog

### RV_20250901

This revision aligns the test specification with the more robust patterns found in the production codebase, focusing on pre-flight validation and consistent architectural patterns.

#### Clean Architectural Refinements
* **Pre-flight Validation:** Added a required test case to ensure `client.Validate()` is called as the first step in `CreateResources`.
    * **Reason:** This enforces a fail-fast approach, preventing wasted API calls and contributing to **L2-2.3 (Resiliency)**.
* **Architectural Consistency:** The method for checking resource existence was shifted from the client (`client.TopicExists()`) to the resource handle (`topic.Exists()`).
    * **Reason:** This aligns the `MessagingManager` with the "get a handle, then act on it" pattern, making the codebase more consistent as per **L2-1.1 (Modularity & Separation of Concerns)**.
* **Idempotent Teardown:** Added a test case to ensure that "not found" errors are ignored during teardown.
    * **Reason:** This makes the teardown operation idempotent, directly fulfilling **L2-1.3 (Idempotency)**.
* **Update Path:** Added a test case to explicitly validate the update path for existing subscriptions.
    * **Reason:** The original prompt was focused on creation and missed specifying the update behavior, which is a key part of **L2-1.3 (Idempotency)**.