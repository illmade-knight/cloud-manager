# **Prompt 04: Implementation for GoogleIAMProjectClient**

**Objective:** Generate the complete implementation for the `GoogleIAMProjectClient`. The generated code must satisfy all previously defined unit and integration tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/googleprojectiam.go` file.

Your implementation must satisfy the unit and integration tests by correctly implementing the safe, non-destructive "get-modify-set" pattern for updating project-level IAM policies and include the security guardrail for the `GrantFirestoreProjectRole` method.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust, production-grade clients for Google Cloud Platform services, with a deep understanding of IAM safety patterns.

**Your Specific Task:** Generate the complete code for `iam/googleprojectiam.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement.
* **(L2-2.2) "Get-Modify-Set" Pattern:** The `AddProjectIAMBinding` method **must** be implemented using the safe get-modify-set pattern. It must:
    1.  Get the current project IAM policy using `client.GetIamPolicy`.
    2.  Programmatically add the new member to the appropriate role binding in the policy object. If the role binding doesn't exist, create it.
    3.  Set the modified policy back on the project using `client.SetIamPolicy`. This ensures no other bindings are accidentally removed.
* **(L2-1.1) Security Guardrail:** The `GrantFirestoreProjectRole` method **must** contain a string prefix check to ensure only roles beginning with `roles/datastore.` can be granted. It must return an error for any other role.
* **Client Usage:** The implementation must use the `cloud.google.com/go/resourcemanager/apiv3` client.
* **Constructor:** Implement a `NewGoogleIAMProjectClient` factory function that correctly initializes the underlying `resourcemanager.ProjectsClient`.

**Dependencies (Full source code to be provided):**

1.  **The Specifications:** `googleprojectiam_test.go` and `googleprojectiam_integration_test.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `GoogleIAMProjectClient` implementation. It places a strong emphasis on security and safety by requiring the non-destructive "get-modify-set" pattern for all project-level policy modifications.