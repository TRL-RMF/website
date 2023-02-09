<p align="center">
 <img src="../images/systems_test_plan.png"/>
    <br>
    <em>Different levels of tests.</em>
</p>

To ensure a stable system integrating multi-party components, different
levels of testing were required. As shown in the diagram above, three
levels of tests were carried out.

Subsequent sub-chapters will go more in depth on the tests carried out
for this project. This section will be dedicated to giving an overview
of the executed tests.

**\
Standalone tests** which were carried out for each major component
involved in this project, namely: RMF, Lifts, Doors and Robot. Each
standalone component test falls under the responsibility of the
individual component vendors. All tests for each component were composed
of different scenarios / cases depending on the project requirement.

For example, for the case of this project, Senserbot tested its robot's
capabilities in crossing the gap at the lift. For RMF's case, the tests
carried out were majorly intensively done in simulation as described in
Chapter 8.

**Integration tests** involve sub-system integrated testing. For the
Tampines Regional Library project and since RMF is the overarching meta
system integrating all components, this involves integrated testing
between:
1. RMF - Lift
2. RMF - Doors
3. RMF - Robots

The tests mainly revolve around testing all actions and feedback sent
and received by RMF with its corresponding subsystem.

**Final tests** are where all sub-systems are integrated and are tested
based on operational scenarios. This also serves as the User Acceptance
Tests for this project.