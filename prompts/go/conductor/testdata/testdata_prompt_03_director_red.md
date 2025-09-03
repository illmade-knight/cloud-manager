# **Prompt 03: Pre-flight Test for ServiceDirector Service**

**Objective:** Generate the pre-flight unit test for the `servicedirector` test data service. This test validates the service's ability to correctly load its complex configuration from multiple embedded YAML files.

**TDD Phase:** Red

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `testdata/servicedirector/main_test.go` file.

Write a unit test for the `ServiceDirector`'s configuration loading logic. The test must create dummy `resources.yaml` and `services.yaml` content and assert that the Pub/Sub configuration (command/completion topics and subscription) is loaded correctly.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `testdata/servicedirector/main_test.go`.

**Key Requirements to Fulfill:**

* **Package:** The test file must be in the `main` package.
* **Test Config Loading:** The test must:
    1.  Programmatically create dummy content for `resources.yaml` that includes the Pub/Sub topics and subscription used by the director.
    2.  Programmatically create dummy content for `services.yaml` that defines the `ServiceManagerSpec` with links to the command and completion topics.
    3.  Call the (not-yet-existing) configuration loading logic from `main.go`.
    4.  Assert that the returned configuration struct is correctly populated with the `CommandTopicID`, `CommandSubID`, and `CompletionTopicID` parsed from the YAML content.

**Dependencies (Full source code to be provided):**

1.  `testdata/servicedirector/main.go`
2.  `microservice/servicedirector/sdconfig.go`
3.  `servicemanager/systemarchitecture.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `servicedirector` service's pre-flight test.