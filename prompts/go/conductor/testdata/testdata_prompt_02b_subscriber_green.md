# **Prompt 02: Implementation for Subscriber Service**

**Objective:** Generate the `main.go` file for the `tracer-subscriber` test data service.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `testdata/tracer-subscriber/main.go` file.

Your implementation must make the pre-flight test in `main_test.go` pass. It must embed and parse `resources.yaml`, load configuration from environment variables, and start a Pub/Sub subscriber that receives messages and republishes them to a verification topic.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `testdata/tracer-subscriber/main.go`.

**Key Requirements to Fulfill:**
* **Pass All Tests.**
* **Configuration:**
    * Use the `//go:embed resources.yaml` directive to embed the configuration file.
    * Implement the `loadAndValidateConfig` function to correctly parse the embedded YAML to find its subscription ID and the verification topic ID it needs to publish to. It must also read runtime config like `PROJECT_ID` from environment variables.
* **Functionality:** The `main` function must:
    1.  Load its configuration.
    2.  Initialize a `pubsub.Client` from the `cloud.google.com/go/pubsub/v2` library.
    3.  Start a subscription receiver using `client.Subscription(...).Receive(...)`.
    4.  In the receiver callback, when a message is received, it must be acknowledged (`m.Ack()`) and its data must be republished to the verification topic. This proves the end-to-end data flow is working.

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `testdata/tracer-subscriber/main_test.go`
2.  `servicemanager/systemarchitecture.go`
3.  `orchestration/configgenerator.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `tracer-subscriber` service implementation.