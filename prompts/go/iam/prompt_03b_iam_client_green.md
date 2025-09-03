# **Prompt 03: Implementation for GoogleIAMClient**

**Objective:** Generate the complete implementation for the `GoogleIAMClient`. The generated code must satisfy all previously defined unit and integration tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/googleiamclient.go` file.

Your implementation must satisfy the `IAMClient` interface by acting as a facade that uses the various underlying Google Cloud SDK clients to manage permissions. It must correctly handle the special additive IAM logic for Cloud Run services and the legacy ACLs for BigQuery Datasets.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust, production-grade clients for Google Cloud Platform services, with a deep understanding of the nuances of its IAM systems.

**Your Specific Task:** Generate the complete code for `iam/googleiamclient.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement.
* **(L2-1.2) Facade Pattern:** The `GoogleIAMClient` struct must be initialized with instances of all necessary underlying clients (`*pubsub.Client`, `*storage.Client`, `*run.Service`, `*admin.IamClient`, etc.). A `NewGoogleIAMClient` factory function must handle this initialization.
* **Multi-Service IAM Logic:** The `ApplyIAMPolicy`, `CheckResourceIAMBinding`, and `RemoveResourceIAMBinding` methods must contain a `switch` statement based on `ResourceType`. Each case must delegate the call to the appropriate underlying client using the correct IAM pattern for that service.
* **(L2-2.2) Specialized IAM Handling:**
    * **Cloud Run:** IAM updates for the `cloudrun_service` type **must be additive**. The implementation must get the existing policy, add the new members to their roles, and then set the modified policy. It must include retry logic to handle eventual consistency delays after a service is created.
    * **BigQuery Datasets:** The implementation for `bigquery_dataset` must correctly interact with the legacy ACL system, translating modern IAM roles to their `bigquery.AccessRole` equivalents for verification and updates.
    * **Firestore:** The implementation must handle `firestore` permissions by applying the roles at the **project level**.
* **Service Account Lifecycle:** Implement `EnsureServiceAccountExists` using the `cloud.google.com/go/iam/admin/apiv1` client, with logic to handle both the "exists" and "not found" cases.

**Dependencies (Full source code to be provided):**

1.  **The Contract:** `iam/iamadapter.go`
2.  **The Specifications:** `googleiamclient_test.go` and `googleiamclient_integration_test.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `GoogleIAMClient` implementation. It incorporates all learnings from the `servicemanager` phase, including the hybrid testing pattern and the need for very specific instructions to handle provider-specific API inconsistencies.