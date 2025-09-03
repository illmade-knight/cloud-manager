# **Prompt 01: Implementation for IAM RolePlanner**

**Objective:** Generate the complete implementation for the `RolePlanner`. The generated code must satisfy the previously defined unit tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/iamplanner.go` file.

I have provided a complete unit test file (`iamplanner_test.go`). Your implementation of the `RolePlanner` **must** make all the tests in that file pass.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in cloud security and Infrastructure as Code.

**Your Specific Task:** Generate the complete code for `iam/iamplanner.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement.
* **Data Structures:** The file must define the exported `IAMBinding` and `PolicyBinding` structs.
* **Planning Logic:** The `PlanRolesForApplicationServices` method must contain all the business logic to correctly interpret the relationships in the `MicroserviceArchitecture` and generate the appropriate `IAMBinding`. This includes logic for:
    * **(L1-2.2) Pub/Sub:** Granting `publisher` and `subscriber` roles based on `ProducerService` and `ConsumerService` links.
    * **(L1-2.2) Data Resources:** Granting `storage.objectAdmin/Viewer`, `bigquery.dataEditor/Viewer`, and `datastore.user/Viewer` roles based on `producers` and `consumers` links.
    * **(L1-2.2) Dependencies:** Granting the `run.invoker` role for service-to-service dependencies.
    * **(L1-2.2) Secrets:** Granting the `secretmanager.secretAccessor` role for services that use `secret_environment_vars`.
* **(L2-1.1) Separation of Concerns:** The planner's logic should be self-contained. It should not perform any validation or have any knowledge of how the plan will be executed; its sole responsibility is to translate the architecture into a list of permissions.
* **Concurrency Safety:** The logic for adding a new binding to the final plan slice must be thread-safe (e.g., using a `sync.Mutex`).

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `iam/iamplanner_test.go`
2.  **The Schema:** `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `RolePlanner` implementation.