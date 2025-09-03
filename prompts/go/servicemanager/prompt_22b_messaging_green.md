# **Prompt 23: Implementation for Pub/Sub Adapter (RV_20250901_CORR3)**

**Objective:** Generate the complete implementation for the `googlemessaging.go` adapter. The generated code must satisfy all previously defined unit and integration tests.

**TDD Phase:** Green (Write the code to make the failing tests pass).

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `servicemanager/googlemessaging.go` file.

I have provided the `MessagingClient` interface and two test files. Your implementation **must** make all tests pass. You **must** use the `cloud.google.com/go/pubsub/v2` client library and its corresponding `.../apiv1/pubsubpb` package for protobuf types.

## **Reinforcement Prompt (Detailed Context)**

**Persona:** You are an expert Go developer specializing in writing robust, production-grade clients for Google Cloud Platform services using the `pubsub/v2` libraries.

**Your Specific Task:** Your task is to generate the complete, full-file code for a single Go file: **`servicemanager/googlemessaging.go`**.

**Key Requirements to Fulfill:**

* **Pass All Tests:** This is the most critical requirement.
* **(L2-1.2) Multi-Layer Adapter Pattern:** You must implement the two-layer adapter pattern to enable unit testing. This involves creating internal, unexported interfaces (e.g., `pubsubTopicAdminClient`) that mirror the Google SDK clients. These internal interfaces **must** have method signatures that exactly match the SDK methods they are abstracting, including variadic `...gax.CallOption` parameters.
* **(L2-1.2) Full Interface Fulfillment:** The `gcpMessagingClientAdapter` must implement **all methods** of the `MessagingClient` interface.
* **(L2-1.2) Type Conversion:** You must implement the helper functions to accurately convert between our generic `SubscriptionConfig` and the Google-specific `pubsubpb.Subscription`.
* **(L2-1.3) Full Update Implementation:** You must implement the `Update` method on the subscription adapter, including the logic for building a `FieldMask`.
* **(L2-2.5) Provider-Specific Validation:** The `Validate` method must check that subscription settings are within the valid ranges defined by Google Cloud Pub/Sub.

**Dependencies (Full source code to be provided):**

1.  **The Contract:** `servicemanager/messagingadapter.go`
2.  **The Specifications:** `googlemessaging_test.go` and `googlemessaging_integration_test.go`

---

## Prompt Changelog

### RV_20250901_CORR3

This revision adds critical corrections to prevent regressions related to package versions, interface fulfillment, and SDK method signatures.

#### Anti-Pattern Corrections
* **Enforced Exact SDK Signatures:** The prompt now requires the internal interfaces (used for mocking) to match the exact signatures of the Google Cloud SDK methods, including variadic `...gax.CallOption` parameters.
    * **Reason (Anti-Pattern Correction):** A previous generation created an internal adapter interface with incorrect method signatures, leading to non-compiling code. The internal interfaces **must** be an exact, verifiable match to the SDK they abstract.
* **Enforced Modern SDK Version & Imports:** The prompt contains a strict instruction to use the `cloud.google.com/go/pubsub/v2` client library and its corresponding `.../apiv1/pubsubpb` package.
    * **Reason (Anti-Pattern Correction):** Prevents the use of outdated or incorrect dependencies.
* **Interface Fulfillment:** The prompt explicitly requires full implementation of the `MessagingClient` interface.
    * **Reason (Anti-Pattern Correction):** A previous generation produced non-compiling code because it failed to implement all required methods. This is a major regression that must be prevented.