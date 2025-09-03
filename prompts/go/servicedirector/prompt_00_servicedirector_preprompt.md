# **Meta-Prompt: ServiceDirector Microservice Generation**

**Persona:** You are an expert Go developer and system architect, specializing in designing and building event-driven, cloud-native microservices.

---
## **1. Component Overview**

We are about to generate the complete Go code for the **`servicedirector` component**. This is a critical piece of the system: a remotely deployed, headless microservice that acts as the hands of the `Conductor`.

Its role is to:
1.  Listen for commands (e.g., "setup dataflow X") on a dedicated Pub/Sub command topic.
2.  Use the `ServiceManager` and `IAMManager` libraries to execute these commands and perform the actual resource provisioning in the cloud.
3.  Publish "completion" or "failure" events to a separate Pub/Sub completion topic when a task is finished.

---
## **2. Methodology & Testing Strategy**

We will follow our standard "Red -> Green -> Refine" TDD workflow. However, the testing strategy for this component is unique:

* **It is an Integration Test, but with Mocks:** The test will validate the service's **transport and command-handling layer** in isolation from its business logic dependencies.
* **Emulator for Transport:** It will use a **Pub/Sub emulator** to simulate the messaging infrastructure.
* **Mocks for Logic:** It will use **mocks** for the downstream dependencies (`ServiceManager`, `IAMManager`).

This strategy allows us to test the core responsibility of the `ServiceDirector`—correctly receiving, parsing, and replying to Pub/Sub messages—without needing to provision real cloud resources during the test, making it fast and reliable.