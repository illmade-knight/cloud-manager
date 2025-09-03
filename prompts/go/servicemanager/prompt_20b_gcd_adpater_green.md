# **Prompt 21: Implementation for GCS Adapter (RV_20250901)**

**Objective:** Generate the complete implementation for the `googlegcs.go` adapter. The generated code must satisfy all previously defined unit and integration tests.

**TDD Phase:** Green (Write the code to make the failing tests pass).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/googlegcs.go` file.

I have provided the `StorageClient` interface and two test files: a unit test (`googlegcs_test.go`) and an integration test (`googlegcs_integration_test.go`). Your implementation of the GCS adapter **must** make all tests in both files pass.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust, production-grade clients for Google Cloud Platform services.

**Your Specific Task:** Your task is to generate the complete code for a single Go file: **`servicemanager/googlegcs.go`**.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement. Your generated code must satisfy both the unit tests (for internal logic) and the integration tests (for lifecycle).
* **(L2-1.2) Multi-Layer Adapter Pattern:** You **must** implement the two-layer adapter pattern from the production code:
    1.  Create an **internal, unexported interface** (e.g., `gcsBucketHandle`) that mirrors the methods of the real `*storage.BucketHandle`.
    2.  Create a **wrapper struct** (e.g., `realGCSBucketHandle`) that implements this internal interface by calling the real SDK.
    3.  Create the **public adapter struct** (e.g., `gcsBucketHandleAdapter`) that holds the *internal interface*, allowing it to be mocked for unit tests.
* **Conversion Logic:** You must implement the `fromGCSBucketAttrs`, `toGCSBucketAttrs`, and `toGCSBucketAttrsToUpdate` functions to handle all type conversions between our generic structs and the GCS SDK structs.
* **(L2-2.3) Robust Error Handling:** You must implement a helper function (e.g., `isGCSBucketNotExist`) to check for multiple "not found" error types from the GCS client. The `Attrs` and `Delete` methods on the public adapter **must** use this helper to translate provider-specific "not found" errors into our generic `servicemanager.ErrBucketNotExist`.

**Dependencies (Full source code to be provided):**

1.  **The Contract:** `servicemanager/storageadapter.go`
2.  **The Specifications:** `googlegcs_test.go` and `googlegcs_integration_test.go`

---

## Prompt Changelog

### RV_20250901

This revision updates the implementation instructions to align with the new Hybrid Testing Strategy, requiring a more sophisticated adapter pattern to enable unit testing of internal logic.

#### Clean Architectural Refinements
* **Multi-Layer Adapter Pattern:** Mandated the implementation of an internal, mockable interface (`gcsBucketHandle`).
    * **Reason:** This pattern is the key enabler for the Hybrid Testing Strategy. It allows the adapter's complex internal logic (type conversions, update diffing) to be unit-tested in isolation from the live SDK, which is a significant improvement in testability and robustness.
* **Error Translation:** Required the implementation of a helper to translate provider-specific errors into our generic `ErrBucketNotExist`.
    * **Reason:** This decouples the rest of the application from the GCS adapter's implementation details, fulfilling **L2-1.2 (Provider Abstraction)**.