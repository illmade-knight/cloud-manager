# **Prompt 03: Implementation for ServiceDirector Service**

**Objective:** Generate the `main.go` file for the `servicedirector` test data service.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `testdata/servicedirector/main.go` file.

Your implementation must make the pre-flight test in `main_test.go` pass. It must embed and parse `resources.yaml` and `services.yaml`, and then instantiate and run the real `ServiceDirector` component.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `testdata/servicedirector/main.go`.

**Key Requirements to Fulfill:**
* **Pass All Tests.**
* **Configuration:**
    * Use `//go:embed resources.yaml` and `//go:embed services.yaml` directives to embed the configuration files.
    * Implement the configuration loading logic to correctly parse both embedded files to discover the Pub/Sub resource names needed by the director.
* **Functionality:** The `main` function is a simple application wrapper. It must:
    1.  Load all necessary configurations from the embedded files and environment variables.
    2.  Instantiate the **real `servicedirector.ServiceDirector` component** from the `microservice/servicedirector` package, passing in the loaded configuration.
    3.  Call the `Start()` method on the `ServiceDirector` instance to run the service.

**Dependencies (Full source code to be provided):**

1.  **The Specification:** `testdata/servicedirector/main_test.go`
2.  `microservice/servicedirector/sdapp.go`
3.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `servicedirector` service implementation.