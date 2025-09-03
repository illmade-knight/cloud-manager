# **Prompt 06: Implementation for TestIAMClient**

**Objective:** Generate a sophisticated test client (`TestIAMClient`) that implements the `IAMClient` interface and manages a pool of service accounts for integration testing.

**TDD Phase:** Green (Direct Implementation)

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/test_iamclient.go` file.

This file will contain a `TestIAMClient` that wraps the real `GoogleIAMClient`. It must override the service account lifecycle methods (`EnsureServiceAccountExists`, `DeleteServiceAccount`) to use a reusable pool, which avoids GCP quota issues during repeated test runs. All other `IAMClient` methods should pass through to the real client.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust and reliable integration test infrastructure for cloud environments.

**Your Specific Task:** Generate the complete code for `iam/test_iamclient.go`.

**Key Requirements to Fulfill:**

* **Interface Fulfillment:** The `TestIAMClient` must fully implement the `IAMClient` interface.
* **Service Account Pooling Logic:**
    * **`NewTestIAMClient`:** The constructor must initialize an internal pool by listing all existing SAs in the GCP project that match a specific prefix (e.g., "it-").
    * **`EnsureServiceAccountExists`:** This method is the core of the pooling logic. It must first try to find a clean, unused SA from its internal pool. If none are available, it should fall back to creating a new one. It must mark the leased SA as "in use".
    * **`DeleteServiceAccount`:** This method must be a "fake" delete. It **must not** delete the SA in GCP. Instead, it must return the SA to the pool by marking it as "not in use".
* **Passthrough Methods:** All other methods (e.g., `ApplyIAMPolicy`, `CheckResourceIAMBinding`) must delegate their calls directly to the wrapped, real `GoogleIAMClient` instance.
* **Cleanup (`Close`):** The `Close` method should perform a cleanup operation, iterating through all SAs used during the test run and removing any IAM policies that were added to them, returning them to a clean state for the next run.

**Dependencies:**

1.  `iam/iamadapter.go` (The interface to implement)
2.  `iam/googleiamclient.go` (The real client to wrap)

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `TestIAMClient`. Its purpose is to generate a critical piece of test infrastructure that makes our integration tests more robust by working around cloud provider API quotas and eventual consistency issues.