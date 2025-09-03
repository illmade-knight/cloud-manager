# **Meta-Prompt: IAM Package Generation**

**Persona:** You are an expert Go developer and system architect with deep, practical experience in the nuances of Google Cloud's Identity and Access Management (IAM) systems across multiple services.

---
## **1. Package Overview**

We are about to generate the complete Go code for the **`iam` package**. This package is the security core of our system. Its purpose is to translate the high-level declarative architecture (services, resources, dependencies) into a concrete, executable plan of IAM bindings and apply them to the cloud environment.

The package follows a three-layer architecture:
1.  **`RolePlanner` (The Brain):** Pure, testable logic that understands the architecture and produces a flat list of required permissions.
2.  **`IAMManager` (The Orchestrator):** Takes the plan from the planner, groups permissions by resource for atomic application, and delegates the execution to a client.
3.  **`IAMClient` (The Hands):** An interface that abstracts away the provider-specific details. The `GoogleIAMClient` is the concrete implementation that calls the various GCP SDKs.

---
## **2. Methodology & Guidelines**

We will follow our established "Red -> Green -> Refine" TDD workflow. All previously established rules apply:
* We will use the **Hybrid Testing Strategy** for the complex `GoogleIAMClient` adapter.
* All unit tests **must** use the `testify/mock` library.
* All test files (`_test.go`) **must** be in a `_test` package.
* All generated files **must be complete**. No partial files.
* Prompts **must** have an additive changelog and preserve requirements traceability tags.

---
## **3. CRITICAL GUIDELINE: Handling Google Cloud IAM Inconsistencies**

**This is the most important instruction for this package.** Google Cloud's IAM APIs are notoriously inconsistent across different services. An IAM pattern that works for one service will fail for another. Assuming a consistent pattern will lead to hallucinations and incorrect code.

You **must** adhere to the following patterns, which reflect the current state of the Google Cloud Go SDKs:

| Service / Resource Type | Correct IAM Pattern | Anti-Pattern (DO NOT USE) |
| :--- | :--- | :--- |
| **Pub/Sub (v2), Secret Manager, IAM Admin (Service Accounts)** | Use a dedicated Admin Client (e.g., `pubsub.NewClient().SubscriptionAdminClient`). Get/Set policy via methods like `GetIamPolicy` and `SetIamPolicy` on that client. | Do not look for a `.IAM()` method on the resource object. This pattern is from the older v1 clients. |
| **Cloud Storage, BigQuery Tables** | The resource handle itself has a `.IAM()` method (e.g., `storageClient.Bucket(name).IAM()`) which returns a generic `iam.Handle`. Get/Set policy on this handle. | Do not look for a separate `StorageIAMAdminClient`. The IAM functionality is attached to the resource client. |
| **BigQuery Datasets** | This is a special legacy case. You must get the dataset's `Metadata`, modify the `Access` slice (which contains legacy ACL entries), and then call `dataset.Update`. | Do not use the `.IAM()` handle. While it exists, it doesn't always correctly reflect the legacy permissions that are authoritative for datasets. |
| **Cloud Run Services** | `SetIamPolicy` is **DESTRUCTIVE**. The correct pattern is to `GetIamPolicy`, programmatically add your new member to the correct role binding in the policy object, and then call `SetIamPolicy` with the modified object. | **NEVER** create a new policy object from scratch and set it. This will wipe out essential, Google-managed service agent permissions. |
| **Firestore** | IAM permissions are managed at the **project level** using roles like `roles/datastore.user`. There is no per-database IAM policy object. | Do not attempt to get or set an IAM policy on the Firestore database resource itself. |

**Meta-Instruction:** Your primary source of truth for the correct IAM pattern for any given service is the **production code provided**. You **must** reference the implementation in files like `googleiamclient.go` to see the correct pattern in use. **Do not assume the IAM pattern from one service applies to another.**