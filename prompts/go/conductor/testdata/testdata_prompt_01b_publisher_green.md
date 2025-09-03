# **Prompt 01: Implementation for Publisher Service**

**Objective:** Generate the `main.go` file for the `tracer-publisher` test data service.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `testdata/tracer-publisher/main.go` file.

Your implementation must make the pre-flight test in `main_test.go` pass. It must embed and parse `resources.yaml` and `tracer-config.yaml`, load configuration from environment variables, and start a Pub/Sub publisher.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `testdata/tracer-publisher/main.go`.

**Key Requirements to Fulfill:**
* **Pass All Tests.**
* **Configuration:**
    * Use the `//go:embed` directive to embed `resources.yaml` and `tracer-config.yaml` into the binary.
    * Implement the `loadAndValidateConfig` function to correctly parse the embedded YAML files and read environment variables.
* **Functionality:** The `main` function should initialize a `pubsub.Client`, start an HTTP server for health checks, and begin publishing messages to the configured topic on a timer to simulate device traffic.