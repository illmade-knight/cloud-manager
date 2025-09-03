# **Plan: TDD for Google Cloud Adapters (RV_20250901)**

This document outlines the testing strategy for our concrete Google Cloud adapters. Our approach varies based on the complexity of the adapter to ensure both correctness and efficiency.

---
## ## Decision Framework

We classify adapters into two categories:

1.  **Simple Adapters:** These are thin wrappers around a cloud SDK with minimal internal logic. They primarily translate method calls directly to the SDK.
2.  **Complex Adapters:** These contain significant internal logic *before* calling the SDK. Examples include complex type conversions, calculating diffs for updates, or conditional logic. The **GCS Adapter** is a prime example due to its attribute conversion functions.

---
## ## Testing Strategies

### Strategy 1: Integration Test Only (For Simple Adapters)

For simple adapters, we will use our original TDD workflow.

1.  **Red Phase (Integration Test):** Generate a `_integration_test.go` file that tests the full resource lifecycle against an emulator.
2.  **Green Phase (Implementation):** Generate the adapter code to make the integration test pass.

### Strategy 2: Hybrid (Unit + Integration) Test (For Complex Adapters)

For complex adapters, a more rigorous, two-stage TDD workflow is required to test both internal logic and external interactions.

1.  **Stage 1: Unit Test the Internal Logic**
    * **Red (Unit):** Generate a `_test.go` file that uses `testify/mock` to test the adapter's *internal logic* (e.g., type conversions, update diffing).
    * **Green (Unit):** Generate just the internal helper functions and type definitions in the adapter file to make the unit tests pass.

2.  **Stage 2: Integration Test the Full Lifecycle**
    * **Red (Integration):** Generate the `_integration_test.go` file that tests the full resource lifecycle against an emulator.
    * **Green (Integration):** Complete the adapter implementation by writing the methods that call the cloud SDK, making the integration tests pass.

This hybrid strategy ensures that complex, internal business logic is tested quickly and in isolation, while the adapter's interaction with the actual cloud service is still verified end-to-end.