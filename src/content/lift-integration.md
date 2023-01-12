# **TRL Lift and Doors Integration Doc**


# Overall System Overview

The objective of lift and doors integration with RMF (Robotics
Middleware Framework) is to facilitate infrastructure interoperability
with the deployed robots in the building.

A physical server, a lift controller box for the service lift and 5 door
controllers for doors on levels 2 through 6 are installed on site to
achieve the aforementioned objective. Refer to figure 1 to get an
overview of the overall architecture.

![](./images/integration_architecture.png){width="5.381767279090114in"
height="5.447398293963254in"}

Figure 1: Physical system architecture.

The server is located at the library's network/server room and it
communicates with AWS via Wi-Fi. A PLC is housed in a lift controller
box at level 6 cargo lift lobby, which is shown in figure 2. It is
connected to the server via LAN/Ethernet cable.

![](./images/lift_lcb.png){width="2.8192672790901137in"
height="2.346673228346457in"}

Figure 2: Lift Controller Box installed on-site (front and side views).

A controller box was installed for each door to control its movement, as
shown in figure 3. The controller box interfaces with the door\'s
internal circuit (pre-installed automated door controller) and motion
sensor as well as its security card access system. An IO acquisition
module is housed in each controller box which is connected to the server
via LAN/Ethernet cable.

![](./images/door_dcb.png){width="6.267716535433071in"
height="3.361111111111111in"}

Figure 3: Door Controller Box installed on-site (the one in photo is at
level 6).

With regards to the particular communication protocol between each
hardware component, RMF communicates with the AWS IoT Core via ROS2
messages which in turn communicates with the server via MQTT . RMF sends
a LiftRequest.msg to AWS to command the lift while it receives
LiftState.msg as lift's status update at a frequency of 1 Hz. Similarly
for doors, with DoorRequest.msg and DoorState.msg.

Server parses through LiftRequest.msg and sends relevant commands to a
PLC (Programmable Logic Controller) via the ADS communication protocol
which is then forwarded to the lift by manipulating corresponding
signals via dry contact. Beckhoff CX9020 was the PLC model used for this
project. It is connected to four digital input terminals and six digital
output terminals..

Similarly, the server parses through DoorRequest.msg and sends relevant
commands to the IO module via RESTful API, which then directly interacts
with the pre-installed automated door controller via a DPDT relay. Adam
6052 was the IO acquisition module used for this project.

The following sections explore each component in detail.

# Integration with Lift

## 2.1 AGV Mode

To facilitate the project's operations, the lift vendor upgraded the
installed lift to accommodate a new mode of operation called AGV mode.
Under this mode of operation, it allowed external hardware to interface
with its internal circuit and control the lift movements.

All new hall calls (buttons pressed from lobby to call lift to that
particular floor) and car calls (floor number buttons pressed from
within lift) are ignored in this mode. Before transitioning into AGV
mode from human/manual mode, all the existing car calls are completed to
ensure passengers already onboard the lift are allowed to get off at
their intended destination floor whereas hall calls are cancelled. This
mode is displayed in the elevator user interface panel as shown in
figure 4 and the same is announced from the speaker as well.

> ![](./images/agv_mode.png){width="2.170708661417323in" height="1.875in"}
>
> Figure 4: The following message: "ATTENTION AUTOMATIC TRANSPORT
> OPERATION" is shown on all car and hall lift displays.

## 2.2 Flow of operations

The standard flow of operations is as follows (with respect to lift
controller): Particular input signals are activated if the lift is in
normal, working condition to transition into AGV mode. After lift
finishes existing car calls, it switches to AGV mode. Lift then moves to
the destination floor as requested. After a successful transit, it stays
at the same floor with its doors kept open until a new command is
received. It exits AGV mode to get to human/manual mode after the robot
has finished its transit.

At a particular floor, a specific signal ensures that lift doors are
open to facilitate robot movement into the lift car. After receiving a
signal to move to another floor, the lift doors are automatically
closed.

![](./images/lift_agv_mode.png){width="3.1771292650918634in"
height="2.3802088801399823in"}

Figure 5: Lift in AGV mode at floor 6 with its doors kept open.

The lift is connected directly to the building's fire alarm signal and
alerts PLC consequently. Lift immediately exits AGV mode (PLC goes into
error state) and proceeds to the first floor and keeps its doors open.
The alarm beacon on top of lift controller box (refer to figure 6) is
enabled which lights up as a red light to alert the nearby patrons and
library staff of the emergency situation.

If the PLC is in error state, all requests are ignored until the
recovery behaviour has been implemented. Section 4 explores all the
possible exception scenarios in more detail.

Please note that as per project requirements, floor B1 is currently
excluded from the operational flow and lift controller box cannot
command the lift to reach it. This can be easily modified later if
needed by making corresponding changes in the lift controller box
hardware and software.

## 2.3 Lift Controller Box

![](./images/lcb_architecture.png){width="4.9162937445319335in"
height="5.161291557305336in"}

Figure 6: Lift Controller Box with respect to the physical system
architecture.

Lift controller box is installed at the sixth floor in the service lift
4 lobby of the library. It looks as shown in figure 6, with different
external components labelled in the image. A conceptual diagram with the
box's contents and its connections to external peripherals and devices
is shown in figure 7.

A turnkey slot is provided to facilitate the library staff to disable or
enable the AGV mode option. If it is toggled towards the Manual mode
option, lift will remain in manual mode and all RMF requests to switch
to AGV mode will be ignored.

A Simulated Fire Alarm button is included to mimic an actual fire alarm.
Its main purpose is to help in the testing of fire alarm exception case
and serves no purpose after deployment. In both simulated and actual
fire, the alarm sound beacon sounds and lights up to alert the nearby
patrons and staff of the emergency.

![](./images/labelled_lcb.png){width="2.771127515310586in"
height="3.2115004374453195in"}

Figure 7: Labelled lift controller box.

![](./images/lcb_conceptual_diagram.png){width="5.939882983377077in"
height="4.265625546806649in"}

Figure 8: Conceptual connections diagram of the lift controller box

# Integration with Doors

## 3.1 Flow of Operations

The standard flow of operations is quite straightforward (with respect
to the door controller): if the door controller is not offline, either
open/close command is received and the door is respectively actuated. In
open command, the door is kept open until explicitly commanded
otherwise. If the door controller is offline, all requests are ignored
until it is back online.

## 3.2 Door Controller Box

![](./media/dcb_architecture.png){width="4.880208880139983in"
height="5.131514654418198in"}

Figure 9: Door Controller Boxes with respect to the physical system
architecture.

Each door has its own door controller box, so there are 5 such boxes
installed on levels 2 through 6. It looks as shown in figure 10, with
different external components labelled in the image. A conceptual
diagram with the box's contents and its connections to external
peripherals and devices is shown in figure 11.

![](./images/labelled_dcb.png){width="6.267716535433071in"
height="2.736111111111111in"}

Figure 10: Labelled door controller box.

![](./images/dcb_conceptual_diagram.png){width="5.75632874015748in"
height="4.741847112860892in"}

Figure 11: Conceptual connections diagram of the door controller box

The limit switches are used as feedback to confirm if the door has
'fully closed'. The pre-installed automated door controller's internal
circuit is utilised to confirm if the door has 'fully opened'.

# Server

![](./images/server_architecture.png){width="4.944267279090114in"
height="5.180242782152231in"}

Figure 12: Server with respect to the physical system architecture.

The application is deployed via a Docker container. It utilises AWS IOT
Core cloud service to connect to the RMF lift adapter. ROS2 messages are
published via MQTT on specific published and subscribed AWS topics.

![](./images/server_software.png){width="4.380208880139983in"
height="3.4634208223972003in"}

Figure 13: Software Architecture of server application.

A LiftState.msg is published to the AWS topic every 2 seconds which is
read by RMF to get all relevant information about the lift state at that
instance. The contents of the message is shown in figure 14. It contains
time of publication, name of lift, available floors (floors which lift
can transit to), lift's current floor (shows actual value only in AGV
mode), lift's destination floor (this target floor was the previous
destination set by RMF), lift door's state (whether it is open, closed
or moving), lift's motion status (moving up, moving down, stopped or
unknown), lift's current mode (offline, manual, AGV, fire or unknown)
and session ID (identification details of the current AGV mode session).

![](./images/lift_state.png){width="5.572916666666667in"
height="7.072916666666667in"}

Figure 14: ListState.msg contents.

The data to be published is retrieved via the PLC using ADS (Automation
Device Specification) communication protocol, which is Beckhoff\'s
preferred method.

RMF commands the lift by publishing a LiftRequest.msg every 1 second on
a different AWS topic which the server subscribes to. The contents of
the message is shown in figure 9. It contains the time of publication,
name of lift, session ID (unique for each requester, in case multiple
agents need to command the lift), request type (whether to request to
AGV mode, manual mode or end the current session), destination floor
(relevant in AGV mode) and lift door state (whether it is open, closed
or moving).

![](./images/lift_request.png){width="6.267716535433071in"
height="5.541666666666667in"}

Figure 15: LiftRequest.msg contents.

A DoorState.msg for each door is published to the respective AWS topic
every 2 seconds which is read by RMF to get all relevant information
about the door state at that instance. The contents of the message is
shown in figure 16. It contains time of publication, name of door, and
the door mode (open, closed or moving).

![](./images/door_state.png){width="2.9471511373578303in"
height="0.8281255468066492in"}

Figure 16: DoorState.msg contents.

RMF commands each door by publishing a DoorRequest.msg every 1 second on
an AWS topic which the server subscribes to. This topic is the same for
all doors. The contents of the message is shown in figure 17. It
contains the time of publication, requester ID (unique for each
requester, in case multiple agents need to command the lift), name of
the door (to specify which door is to be actuated), and requested mode
(whether to request to open or close).

![](./images/door_request.png){width="3.369792213473316in"
height="1.0935618985126858in"}

Figure 17: DoorRequest.msg contents.

# Exception Scenarios

The following subsections explore all possible exception cases for lift
and doors and their respective recovery behaviours. The application is
built in a non-blocking way, for example, if one door has error or is
offline, other doors and lift are unaffected and vice-versa.

## 5.1 Lift Exceptions and Recovery Behaviour

Table 1 summarises the exception scenarios possible with regards to the
lift and its controller box. It also specifies the corresponding
recovery behaviour to handle the occurred exception.

> Table 1: Exception Scenarios for service lift:

+-----+------------------+------------------------+-------------------+
| **  | **E              | **Action**             | **Recovery        |
| Sr. | rror/Exception** |                        | behaviour**       |
| No  |                  |                        |                   |
| .** |                  |                        |                   |
+=====+==================+========================+===================+
| 1   | Loss of          | 1.  Lift goes back to  | When the          |
|     | communications   |     > manual mode.     | heartbeat is      |
|     | between server   |                        | heard again, it   |
|     | and PLC.         | 2.  PLC state goes to  | goes back to      |
|     |                  |     > error state.     | human mode.       |
|     |                  |                        |                   |
|     |                  |                        | No need for human |
|     |                  |                        | intervention.     |
+-----+------------------+------------------------+-------------------+
| 2   | Fire Alarm is    | 1.  Lift goes back to  | Manual            |
|     | activated.       |     > manual mode.     | intervention is   |
|     |                  |                        | needed. PLC will  |
|     |                  | 2.  PLC state goes to  | stay in error     |
|     |                  |     > error state.     | state until it is |
|     |                  |                        | restarted.        |
|     |                  | 3.  Sound beacon is    |                   |
|     |                  |     > switched on.     |                   |
+-----+------------------+------------------------+-------------------+
| 3   | Lift is not in   | 1.  Lift goes back to  | No need for human |
|     | operational      |     > manual mode.     | intervention.     |
|     | state.           |                        | Operations will   |
|     |                  | 2.  PLC state goes to  | continue as they  |
|     |                  |     > error state.     | were after lift   |
|     |                  |                        | is back in        |
|     |                  |                        | operational       |
|     |                  |                        | state.            |
+-----+------------------+------------------------+-------------------+
| 4   | Simulated Fire   | 1.  Lift goes back to  | Manual            |
|     | Alarm button is  |     > manual mode.     | intervention is   |
|     | pressed.         |                        | needed. PLC will  |
|     |                  | 2.  PLC state goes to  | stay in error     |
|     |                  |     > error state.     | state until it is |
|     |                  |                        | restarted.        |
|     |                  | 3.  Alarm beacon is    |                   |
|     |                  |     > switched on.     |                   |
+-----+------------------+------------------------+-------------------+
| 5   | If turnkey is    | 1.  Lift goes back to  | To return things  |
|     | toggled towards  |     > manual mode.     | to normal, toggle |
|     | Manual           |                        | turnkey towards   |
|     |                  | 2.  PLC state goes to  | AGV mode.         |
|     |                  |     > error state      |                   |
+-----+------------------+------------------------+-------------------+
| 6   | Mode Timeout (10 | 1.  Lift goes back to  | Manual            |
|     | minutes)         |     > manual mode.     | intervention is   |
|     |                  |                        | needed. PLC will  |
|     | (Lift is unable  | 2.  PLC state goes to  | stay in error     |
|     | to go to AGV     |     > error state.     | state until it is |
|     | mode from human  |                        | restarted.        |
|     | mode)            |                        |                   |
+-----+------------------+------------------------+-------------------+
| 7   | Destination      | 1.  Lift goes back to  | Operations flow   |
|     | floor requested  |     > manual mode.     | is not hampered.  |
|     | is not in        |                        | RMF to send a     |
|     | available floors |                        | correct           |
|     |                  |                        | destination floor |
|     |                  |                        | in the next       |
|     |                  |                        | request.          |
+-----+------------------+------------------------+-------------------+
| 8   | Lift movement    | 1.  Lift goes back to  | Manual            |
|     | timeout - Lift   |     > manual mode.     | intervention is   |
|     | did not reach    |                        | needed. PLC will  |
|     | destination      | 2.  PLC state goes to  | stay in error     |
|     | floor after 2    |     > error state.     | state until it is |
|     | minutes          |                        | restarted.        |
+-----+------------------+------------------------+-------------------+

If PLC goes into error state, it is indicated by the last LED on its
output terminal, as shown in figure 18:

![](./images/plc_error.png){width="4.291666666666667in"
height="2.8645833333333335in"}

Figure 18: Error state indication by LED on PLC, encircled by red
ellipse.

## 5.2 Door Exceptions and Recovery Behaviour

Table 2 summarises the exception scenarios possible with regards to the
lift and its controller box. It also specifies the corresponding
recovery behaviour to handle the occurred exception.

> Table 2: Exception Scenarios for doors:

+-----+------------------+------------------------+-------------------+
| **  | **E              | **Action**             | **Recovery        |
| Sr. | rror/Exception** |                        | behaviour**       |
| No  |                  |                        |                   |
| .** |                  |                        |                   |
+=====+==================+========================+===================+
| 1   | Loss of          | 1.  Door mode is       | Need for human    |
|     | communications   |     > reflected as     | intervention if   |
|     | between server   |     > 'offline'.       | it stays          |
|     | and door         |                        | disconnected for  |
|     | controller.      |                        | a long time.      |
|     |                  |                        | After             |
|     |                  |                        | reconnection, the |
|     |                  |                        | operation         |
|     |                  |                        | continues as      |
|     |                  |                        | before.           |
+-----+------------------+------------------------+-------------------+
| 2   | Cannot detect    | > \-                   | Need for manual   |
|     | fully open or    |                        | intervention to   |
|     | fully closed     |                        | fix the relevant  |
|     | door state.      |                        | sensor.           |
+-----+------------------+------------------------+-------------------+