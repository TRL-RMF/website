
# Standalone test - Lifts and Doors

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

<p align="center">
Table 1: Tests for components for lift integration
</p>

|                                          |                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|*Components*|*Categories*|*Test Cases*|*Reasons*|
| Retrofitted Electronics from lift vendor | Validation Test    | Testing validity of “lift in operation” feedback signal. <br/> <br/> Reservation of lift. <br/> <br/> Release of lift. <br/> <br/> Calling lift to go to a level.                                                                                                                                                                                                                                                                                                                                         | This test is critical as it serves the following purposes: <br/> 1. Ensure installation and enhancement <br/> works from the lift vendor are done correctly. <br/> 2. Ensure the handovered system works as <br/> intended to pre-given specification documents. <br/> The design of the developed lift controller box references these specification documents. <br/> A test early on will show any discrepancy in provided information which may affect the final developed design. |
| Lift Controller Box                      | Functionality Test | Testing reading of “lift in operation” feedback signal. <br/><br/> Reservation of lift. <br/><br/> Release of lift. <br/><br/> Calling lift to go to a level. <br/><br/> Exceptional Behaviors:  <br/> - Turnkey to non-interfering mode <br/> - Electrical release of signals in error / exception mode <br/> - Reconnecting when loss of 2 way heartbeat <br/> - And more <br/> <br/> Quality checks <br/> - Thermal ventilation <br/> - Crimping and connection quality <br/> - Mounting reliabilities | To ensure the developed system is assembled <br/> and fabricated as designed.To ensure fabricated <br/> electronics are stable, safe and durable for 24/7 <br/> operations.                                                                                                                                                                                                                                                                                                           |
|                                          | Software Test      | Static Analysis Tests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Ensure production quality code. <br/> Expose vulnerabilities in “distance” & “hard-to-reach” code.                                                                                                                                                                                                                                                                                                                                                                                    |



**Standalone tests for door integration**

Similar to the standalone tests done for lift integrations, the
developed door controllers also follow similar test cases to ensure its
reliability.

A more in-depth explanation on the work behind door integrations can be
found in Chapter 10.2.2.

<p align="center">
Table 2: Tests for components for door integration
</p>

|                                          |                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|*Components*|*Categories*|*Test Cases*|*Reasons*|
| Door Controller Box | Functionality Test | Testing reading of door states<br/><br/>Testing of commanding doors to close and open. <br/><br/>Exceptional Behaviors <br/>- Switch to non-interfering mode <br/>- Connection loss behaviors <br/>- And more<br/><br/>Quality checks | To ensure the developed system is assembled and fabricated as designed.<br/><br/>To ensure fabricated electronics are stable, safe and durable for 24/7 operations. |
|                     | Software Test      | Static Analysis Tests                                                                                                                                                                                                                 | Ensure production quality code. <br/> Expose vulnerabilities in “distance” & “hard-to-reach” code.                                                                  |
