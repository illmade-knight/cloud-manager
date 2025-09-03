## red - green - refine - refactor - repeat

Our requirements to code process currently has an extensive - untracked 
refinement phase.

After we generate tests (red) we then generate code to pass the tests (green).

For simple code this is often sufficient, however, 
significant sections of code require refinement. 
Here we examine non-compiling code, use of outdated api or older coding practices etc...
The prompt-code-prompt-code cycle can be lengthy and inline capture of this is not terribly useful.

We want a process that takes the end state of this process and captures, succinctly,
the updates to our original prompts required to generate the end code.

### proposals

Create batches of red-green prompts to run - the initial state is recorded.
Iterate until the code is clean, up to date works.

Compare initial state red-green with final state red-green

Require LLM to refine red-green prompts until generated state matches final state

Separate 'clean' prompt surfaces from 'fix' prompt surfaces. 
The clean state should reflect our ideal prompt that our perfect LLM uses to generate the final state
The fix states represent the reality of the additional information required to achieve the goal.

By separating our states we allow pruning of fix states as LLM capabilities and knowledge base improve.



