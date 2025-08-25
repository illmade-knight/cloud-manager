# cloud-manager
Infrastructure as code via prompts

## moving on from experiments

This repo attempts to create maintainable prompt driven library generation

We move from a [plan](requirements_plan.md) to [requirements](reqirements)  

For specific languages, [go](prompts/go) to start with, we adopt a deep context building
approach with [meta](prompts/go/meta_prompt.md) prompting to build a persona as well as specific
go [guidelines](prompts/go/golang_codestyle.md) for the LLM to use.

There are a large number of prompts to be applied according to a [human](prompts/go/human_delivery) based
delivery plan. The long term goal is to automate generation but there is still significant human-in-the-loop presence
required for of TDD red-green-refactor loops.
