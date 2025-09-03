# **Prompt 07: Unit Test for GrantCloudRunAgentPermissions**

**Objective:** Generate a unit test for the `GrantCloudRunAgentPermissions` helper function.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/iamroles_test.go` file.

Using `testify/mock`, write a unit test for the `GrantCloudRunAgentPermissions` function. The test must cover the cases where the permission already exists and where it needs to be added.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer with deep experience in writing thorough unit tests using `testify/mock`.

**Your Specific Task:** Generate the complete code for `iam/iamroles_test.go`.

**Key Requirements to Fulfill:**

* **Mocking:** You must mock the `IAMClient` and the underlying `artifactregistry.Client` (specifically its `GetIamPolicy` and `SetIamPolicy` methods).
* **Test Coverage:**
    * **Permission Exists Case:** Write a test where the mock `GetIamPolicy` returns a policy that already contains the required Cloud Run agent member and role. Assert that `SetIamPolicy` is **not** called and the function returns `(false, nil)`.
    * **Permission Missing Case:** Write a test where `GetIamPolicy` returns a policy *without* the required binding. Assert that `AddArtifactRegistryRepositoryIAMBinding` **is** called and the function returns `(true, nil)`.

**Dependencies:**

1.  `iam/iamroles.go`
2.  `iam/iamadapter.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `iamroles` helper function tests.