# **Prompt 07: Implementation for GrantCloudRunAgentPermissions**

**Objective:** Generate the implementation for the `GrantCloudRunAgentPermissions` helper function.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/iamroles.go` file.

I have provided a unit test in `iamroles_test.go`. Your implementation of `GrantCloudRunAgentPermissions` must make the tests pass by correctly checking for and adding the `artifactregistry.reader` role for the Cloud Run service agent.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in cloud security and Infrastructure as Code.

**Your Specific Task:** Generate the complete code for `iam/iamroles.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **(L2-1.3) Idempotency:** The implementation must be idempotent. It must first get the current IAM policy for the Artifact Registry repository, check if the Cloud Run service agent member already has the `roles/artifactregistry.reader` role, and **only** proceed to add the binding if it is missing.
* **Logic:** The function must correctly construct the Cloud Run service agent's email address from the provided project number. It must then call the appropriate methods on its client dependencies to perform the check-and-set operation.

**Dependencies:**

1.  **The Specification:** `iam/iamroles_test.go`
2.  **The Contract:** `iam/iamadapter.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `iamroles` helper function.