### **\#\# Orchestration Package: Refactor Plan**

Our goal is to create a full suite of robust, refined prompts for the orchestration package and its associated testdata services. We will apply our mature TDD methodology to each individual component.

#### **Files to Produce**

**1\. Orchestration Prompts (orchestration/)**

* **Prompt prompt\_00\_orchestration\_preprompt.md:** The master context for the package.
* **Prompt prompt\_01\_config\_generator\_red.md:** "Red" prompt for the ConfigGenerator unit test.
* **Prompt prompt\_01b\_config\_generator\_green.md:** "Green" prompt for the ConfigGenerator implementation.
* **Prompt prompt\_02\_preflight\_validator\_red.md:** "Red" prompt for the PreflightValidator unit test.
* **Prompt prompt\_02b\_preflight\_validator\_green.md:** "Green" prompt for the PreflightValidator implementation.
* **Prompt prompt\_03\_deployment\_orchestrator\_red.md:** "Red" prompt for the DeploymentManager unit test.
* **Prompt prompt\_03b\_deployment\_orchestrator\_green.md:** "Green" prompt for the DeploymentManager implementation.
* **Prompt prompt\_04\_iam\_orchestrator\_red.md:** "Red" prompt for the IAMOrchestrator integration test.
* **Prompt prompt\_04b\_iam\_orchestrator\_green.md:** "Green" prompt for the IAMOrchestrator implementation.
* **Prompt prompt\_05\_director\_client\_red.md:** "Red" prompt for the RemoteDirectorClient integration test.
* **Prompt prompt\_05b\_director\_client\_green.md:** "Green" prompt for the RemoteDirectorClient implementation.
* **Prompt prompt\_06\_conductor\_red.md:** "Red" prompt for the Conductor unit test.
* **Prompt prompt\_06b\_conductor\_green.md:** "Green" prompt for the Conductor implementation.

**2\. Test Data Prompts (testdata/)**

* **Prompt testdata\_prompt\_01\_publisher\_red.md:** "Red" prompt for the publisher's pre-flight test.
* **Prompt testdata\_prompt\_01b\_publisher\_green.md:** "Green" prompt for the publisher's main.go.
* **Prompt testdata\_prompt\_02\_subscriber\_red.md:** "Red" prompt for the subscriber's pre-flight test.
* **Prompt testdata\_prompt\_02b\_subscriber\_green.md:** "Green" prompt for the subscriber's main.go.
* **Prompt testdata\_prompt\_03\_director\_red.md:** "Red" prompt for the director's pre-flight test.
* **Prompt testdata\_prompt\_03b\_director\_green.md:** "Green" prompt for the director's main.go.