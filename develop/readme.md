## developing cloud-manager

At any point in time the cloud-manager exists as a living collection of libraries

Development of cloud-manager can occur from *any* point. What does this mean?

* A change can be made to high level requirements
* A change can be made to a prompt
* A change can be made to code

### Surely we should make a change to requirements first?

We believe a change to code is as valid an update to our project as a change to requirements.

Each section is an independent *idea factory*. 

While writing code we may, intentionally or unintentionally be modifying requirements - this is OK - as long as change is
recognized and propagates.

Prompts can be seen as an idea factory to fulfill requirements but this doesn't mean the ideas generated in prompts cannot 
expose weaknesses in the requirements or serve to expand the scope of requirements.

Changes to different parts of our system have different effects.

* a change to requirements *must* change our prompts and our code
* a change to prompts *should* change requirements and *must* change code
* a change to code *should* be reflected in changes to prompts and changes to requirements

### Keeping track 

Any change needs to be verified. If we think in terms of code, when we want our changes to be merged we run our tests:
in our new environment these tests expand upwards - we verify the prompts reflect the changes and the changes to prompts are consistent.
We see if the changes affect requirements, if they do these changes need to be approved.

If we change code then we climb back up the change
* does the change affect prompts: it should in high fidelity prompts but may not for lower fidelity prompts
* requirements: many coding changes will *not* affect requirements

If we change a prompt
* code *should* update
* requirements: many prompt changes will *not* affect requirements

If we change a requirement 
* prompts *should* update
* code *should* update

The processes to change and accept the changes 
