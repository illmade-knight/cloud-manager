### **IAM Package: Plan of Action**

Our goal is to create a full suite of robust, refined prompts for the iam package, following our established TDD methodology.

#### **Files to Produce**

1. **Prompt Files:**
    * prompt\_00\_iam\_preprompt.md: (This file) The master context for the entire package, outlining the architecture and critical guidelines for handling Google IAM inconsistencies.
    * prompt\_07\_iam\_planner\_red.md: The "Red" prompt to generate the unit tests for the RolePlanner.
    * prompt\_07b\_iam\_planner\_green.md: The "Green" prompt to generate the RolePlanner implementation.
    * prompt\_08\_iam\_manager\_red.md: The "Red" prompt to generate the unit tests for the IAMManager.
    * prompt\_08b\_iam\_manager\_green.md: The "Green" prompt to generate the IAMManager implementation.
    * prompt\_09\_iam\_client\_red.md: The "Red" prompt for the concrete GoogleIAMClient, specifying the hybrid (unit \+ integration) test suite.
    * prompt\_09b\_iam\_client\_green.md: The "Green" prompt for the GoogleIAMClient implementation.
2. 
3. **Generated Go Code Files:**
    * iamplanner.go: The pure logic for translating an architecture into a list of required permission bindings.
    * iamplanner\_test.go: The unit tests for the planner.
    * iammanager.go: The orchestration logic for applying an IAM plan.
    * iammanager\_test.go: The unit tests for the manager.
    * googleiamclient.go: The concrete adapter implementation for Google Cloud IAM.
    * googleiamclient\_test.go: The unit tests for the adapter's internal logic.
    * googleiamclient\_integration\_test.go: The integration tests for the adapter's live API interactions.
    
