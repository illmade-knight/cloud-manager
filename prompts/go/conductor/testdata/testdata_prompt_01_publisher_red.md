# **Prompt 01: Pre-flight Test for Publisher Service**

**Objective:** Generate the pre-flight unit test for the `tracer-publisher` test data service. This test validates the service's ability to correctly load its configuration.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `testdata/tracer-publisher/main_test.go` file.

Write a unit test for the publisher's `loadAndValidateConfig` function. The test must simulate the pre-flight check by creating dummy `resources.yaml` and `tracer-config.yaml` files and setting environment variables, then asserting that the configuration is loaded correctly.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `testdata/tracer-publisher/main_test.go`.

**Key Requirements to Fulfill:**

* **Package:** The test file must be in the `main` package.
* **Test `loadAndValidateConfig`:** The test must:
    1.  Programmatically create dummy content for `resources.yaml` that includes a `TopicConfig` linked to this service.
    2.  Programmatically create a dummy `tracer-config.yaml`.
    3.  Set the required `PROJECT_ID` environment variable using `t.Setenv`.
    4.  Call the (not-yet-existing) `loadAndValidateConfig` function from `main.go`.
    5.  Assert that the returned config struct is correctly populated with values from the dummy YAML content and the environment variable.