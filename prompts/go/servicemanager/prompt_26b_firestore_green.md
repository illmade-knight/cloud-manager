# **Prompt 27: Implementation for Firestore Adapter (RV_20250901)**

**Objective:** Generate the complete implementation for the `googlefirestore.go` adapter. The generated code must satisfy the previously defined integration tests.

**TDD Phase:** Green (Write the code to make the failing tests pass).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/googlefirestore.go` file.

Your implementation **must** use the aliased `cloud.google.com/go/firestore/apiv1/admin` client for all management operations. The `Delete` method must be a non-functional placeholder that returns an error.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust, production-grade clients for Google Cloud Platform services.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/googlefirestore.go`**.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement.
* **(L2-1.2) Use Correct Client & Aliases:** You **must** use explicit import aliases for the Firestore Admin client and its protobuf types to avoid name collisions (e.g., `firestoreadmin`, `firestoreadminpb`). The implementation must use this admin client for all operations.
* **`Exists` Method:** Implement by calling the admin client's `GetDatabase` method and correctly handling `codes.NotFound` gRPC errors.
* **`Create` Method:** Implement by calling the admin client's `CreateDatabase` method. This is a long-running operation, and you **must** wait for it to complete.
* **Safe `Delete` Method:** The `Delete` method on the `gcpDatabaseHandleAdapter` **must not** attempt to delete the database. Instead, it must return a descriptive error stating that programmatic deletion is not supported.

**Dependencies (Full source code to be provided):**

1.  **The Contract:** `servicemanager/documentstore_adapter.go`
2.  **The Specification:** `servicemanager/firestoremanager_integration_test.go`

---

## Prompt Changelog

### RV_20250901

This revision updates the implementation instructions with best practices observed in the production code.

#### Clean Architectural Refinements
* **Explicit Import Aliases:** The prompt now mandates the use of import aliases for the Firestore Admin client and its protobufs.
    * **Reason:** This is a best practice that improves code clarity and prevents subtle bugs from name collisions between different client libraries (e.g., admin vs. data-plane).
* **Safe Delete Operation:** The `Delete` method on the adapter handle is now required to return an error instead of being a no-op.
    * **Reason:** This creates a safer, more explicit contract. It prevents developers from incorrectly assuming that the method will delete the database, which is a protected operation. This is a better fulfillment of **L2-2.1 (Lifecycle Protection)**.