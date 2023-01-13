>![](../images/systems_test_plan.png)

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
between:\
1. RMF - Lift\
2. RMF - Doors\
3. RMF - Robots

The tests mainly revolve around testing all actions and feedback sent
and received by RMF with its corresponding subsystem.

**Final tests** are where all sub-systems are integrated and are tested
based on operational scenarios. This also serves as the User Acceptance
Tests for this project.

9.1 Static Lift Rig Tests

9.2 Standalone test - Lifts and Doors

Apart from the multi-level design reviews carried out for the developed
components for lift and door integration, a list of tests have also been
carried out on the standalone level to ensure quality.

**Standalone tests for lift integration**

To respect the confidentiality of the lift vendor who provided its
services and helping hands in this project, the details of the conducted
tests will not be disclosed in this section. However, this section will
still give a description of the tested items / cases to provide a better
understanding of what needs to be tested to ensure quality and
stability.

For RMF to control the lift, the lift vendor has retrofitted its service
lift with electronics which exposes its functionality to external
systems via dry contact interfaces. A beckhoff PLC coupled with
additional electronics (Lift Controller Box) which holds the logic in
firing the correct sequence of signals to the lift vendor's electronics
serves as a safe and stable intermediate controller. Higher level
software calls can then be routed to the PLC to control the lift. A more
detailed description of the work in integrating the lift can be found in
Chapter 10.2.2.

Table 1 below shows a high level description of the tests carried out
for the components for lift integration.

>Table 1: Tests for components for lift integration
>

|                                          |                    |                                                                                                                                                                                                                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------------------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Components                               | Categories         | Test Cases                                                                                                                                                                                                                                                                                                                                                                                     | Reasons                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Retrofitted Electronics from lift vendor | Validation Test    | Testing validity of “lift in operation” feedback signal. <br/> <br/> Reservation of lift. <br/> <br/> Release of lift. <br/> <br/> Calling lift to go to a level.                                                                                                                                                                                                                                                                     | This test is critical as it serves the following purposes: <br/> 1. Ensure installation and enhancement works from the lift vendor are done correctly. <br/> 2. Ensure the handovered system works as intended to pre-given specification documents. The design of the developed lift controller box references these specification documents. A test early on will show any discrepancy in provided information which may affect the final developed design. |
| Lift Controller Box                      | Functionality Test | Testing reading of “lift in operation” feedback signal. <br/><br/> Reservation of lift. <br/><br/> Release of lift. <br/><br/> Calling lift to go to a level. <br/><br/> Exceptional Behaviors: <br/> - Turnkey to non-interfering mode <br/> - Electrical release of signals in error / exception mode <br/> - Reconnecting when loss of 2 way heartbeat <br/> - And more <br/> <br/> Quality checks <br/> - Thermal ventilation <br/> - Crimping and connection quality <br/> - Mounting reliabilities | To ensure the developed system is assembled and fabricated as designed.To ensure fabricated electronics are stable, safe and durable for 24/7 operations.                                                                                                                                                                                                                                                                                        |
|                                          | Software Test      | Static Analysis Tests                                                                                                                                                                                                                                                                                                                                                                          | Ensure production quality code. <br/> Expose vulnerabilities in “distance” & “hard-to-reach” code.                                                                                                                                                                                                                                                                                                                                                     |



**Standalone tests for door integration**

Similar to the standalone tests done for lift integrations, the
developed door controllers also follow similar test cases to ensure its
reliability.

A more in-depth explanation on the work behind door integrations can be
found in Chapter 10.2.3.

>Table 2: Tests for components for door integration
>

|                     |                    |                                                                                                                                                                                        |                                                                                                                                                           |
| ------------------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Components          | Categories         | Test Cases                                                                                                                                                                             | Reasons                                                                                                                                                   |
| Door Controller Box | Functionality Test | Testing reading of door states<br/><br/>Testing of commanding doors to close and open. <br/><br/>Exceptional Behaviors <br/>- Switch to non-interfering mode <br/>- Connection loss behaviors <br/>- And more<br/><br/>Quality checks | To ensure the developed system is assembled and fabricated as designed.<br/><br/>To ensure fabricated electronics are stable, safe and durable for 24/7 operations. |
|                     | Software Test      | Static Analysis Tests                                                                                                                                                                  | Ensure production quality code. <br/> Expose vulnerabilities in “distance” & “hard-to-reach” code.                                                              |

**Integration tests - RMF-Lift and RMF-Doors**

On the second level, integration tests between the Lift as well as Doors
and RMF were carried out to ensure integration quality and correctness
as well as expose any shortcomings or bugs in terms of behavioral
mismatches between the two systems.

A more in depth explanation of the architecture between RMF and the
infrastructure systems can be found in Chapter 10.2.2 and 10.2.3.

>Table 3: Tests for integration of RMF-Lifts and RMF-Doors
>

|                    |                          |                                                                                                                                                                                                                                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------ | ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Subsystems         | Categories               | Test Cases                                                                                                                                                                                                                                                                                                                 | Reasons                                                                                                                                                                                                                                                                                                                                                           |
| RMF-Lift           | Functionality Test       | Changing of modes: AGV mode, Human mode.<br/><br/>Calling of lift to level 2,3,4,5,6.<br/><br/>Reading and publishing of lift states for all modes and all floors.<br/><br/>Exceptional behaviors <br/>- Loss of comms <br/>- Fire Alarm mode <br/>- Timeouts -<br/> Non-interfering mode <br/>- And more<br/><br/>Mechanical quality checks<br/>- Lift controller box mounting <br/>- Dust protection | To ensure integration is done correctly. <br/><br/> To ensure the system is operational even during exceptional situations. <br/><br/> To ensure no unintended “hanging” states for all situations. <br/><br/> To ensure the system is recoverable after exceptional situations. <br/><br/> To ensure the lift controller box is installed in a safe and reliable manner.                                         |
| RMF-Doors          |                          | Commanding of doors <br/><br/> Reading and publishing door states <br/><br/> Exceptional behaviors<br/>- Loss of comms <br/>- Fire Alarm mode <br/>- Timeouts <br/>- Non-interfering mode <br/>- And more<br/><br/>Mechanical quality checks<br/>- Door controller box mounting<br/> - Sensor mounting reliability <br/>- Dust protection                                                             | To ensure integration is done correctly.<br/><br/>To ensure the system is operational even during exceptional situations.<br/><br/>To ensure no unintended “hanging” states for all situations.<br/><br/>To ensure the system is recoverable after exceptional situations.<br/><br/>To ensure the lift controller box is installed in a safe and reliable manner.                                         |
| RMF- LiftRMF-Doors | Connectivity Stress Test | Extended period of packet drop monitoringValidating no single point of failure                                                                                                                                                                                                                                             | To ensure the reliability of the connectivity. <br/><br/> Due to circumstances, the infrastructure-rmf software adapter for all integrated doors and lifts is a monolithic piece of code multithreaded into different processes. This test also covers the code that never blocks / crashes due to a single point of hardware failure in any of the door or lift controllers. |


**\
Final Test**

This is the test where all components including Senserbot, RMF and the
integrated lifts and doors are integrated.

For this project, a total of three scenarios (which represent similar
operational situations) were tested. The tested scenarios are the
following:

1.  Robot moving through lift and door to workcell to perform its mission

    a.  To demonstrate the ability of robots to take lift to different floors, open and move through doors and start its task.

    b.  Consists of 20 permutations of robots taking lift from Floor A to Floor B and opening the doors for both Floor A and Floor B.

2.  Wireless Charging of Robot

    a.  To demonstrate the robot can be commanded by RMF to go and get charged at level 6 wireless charging point from any floor.

    b.  Consists of 5 permutations of the robot taking lift from Floor X to Floor 6's charging point.

3.  Robot's movement during Emergency

    a.  To demonstrate that during an emergency situation such as a building fire alarm being triggered, the robots can automatically move out of the main paths of evacuation to prevent blockage. It also covers recovery behaviors of the robot when the emergency situation is disengaged.

    b.  Consists of 5 permutations of the robot traveling when a fire alarm is triggered.

The test plans for the above scenarios can be downloaded here.