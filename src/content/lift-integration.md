# **TRL Lift and Doors Integration Doc**


1. Overall System Overview

The objective of lift and doors integration with RMF (Robotics Middleware Framework) is to facilitate infrastructure interoperability with the deployed robots in the building.

A physical server, a lift controller box for the service lift and 5 door controllers for doors on levels 2 through 6 are installed on site to achieve the aforementioned objective. Refer to figure 1 to get an overview of the overall architecture.  

Figure 1: Physical system architecture.

The server is located at the library’s network/server room and it communicates with AWS via Wi-Fi. A PLC is housed in a lift controller box at level 6 cargo lift lobby, which is shown in figure 2. It is connected to the server via LAN/Ethernet cable.



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")


Figure 2: Lift Controller Box installed on-site (front and side views).

A controller box was installed for each door to control its movement, as shown in figure 3. The controller box interfaces with the door's internal circuit (pre-installed automated door controller) and motion sensor as well as its security card access system. An IO acquisition module is housed in each controller box which is connected to the server via LAN/Ethernet cable.



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")


Figure 3: Door Controller Box installed on-site (the one in photo is at level 6).

With regards to the particular communication protocol between each hardware component, RMF communicates with the AWS IoT Core via ROS2 messages which in turn communicates with the server via MQTT . RMF sends a LiftRequest.msg to AWS to command the lift while it receives LiftState.msg as lift’s status update at a frequency of 1 Hz. Similarly for doors, with DoorRequest.msg and DoorState.msg.

Server parses through LiftRequest.msg and sends relevant commands to a PLC (Programmable Logic Controller) via the ADS communication protocol which is then forwarded to the lift by manipulating corresponding signals via dry contact. Beckhoff CX9020 was the PLC model used for this project. It is connected to four digital input terminals and six digital output terminals..

Similarly, the server parses through DoorRequest.msg and sends relevant commands to the IO module via RESTful API, which then directly interacts with the pre-installed automated door controller via a DPDT relay. Adam 6052 was the IO acquisition module used for this project.

The following sections explore each component in detail.



2. Integration with Lift


## 2.1 AGV Mode

To facilitate the project’s operations, the lift vendor upgraded the installed lift to accommodate a new mode of operation called AGV mode. Under this mode of operation, it allowed external hardware to interface with its internal circuit and control the lift movements. 

All new hall calls (buttons pressed from lobby to call lift to that particular floor) and car calls (floor number buttons pressed from within lift) are ignored in this mode. Before transitioning into AGV mode from human/manual mode, all the existing car calls are completed to ensure passengers already onboard the lift are allowed to get off at their intended destination floor whereas hall calls are cancelled. This mode is displayed in the elevator user interface panel as shown in figure 4 and the same is announced from the speaker as well.


    

<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")



    Figure 4: The following message: “ATTENTION AUTOMATIC TRANSPORT OPERATION” is shown on all car and hall lift displays.


## 2.2 Flow of operations

The standard flow of operations is as follows (with respect to lift controller): Particular input signals are activated if the lift is in normal, working condition to transition into AGV mode. After lift finishes existing car calls, it switches to AGV mode. Lift then moves to the destination floor as requested.  After a successful transit, it stays at the same floor with its doors kept open until a new command is received. It exits AGV mode to get to human/manual mode after the robot has finished its transit.

At a particular floor, a specific signal ensures that lift doors are open to facilitate robot movement into the lift car. After receiving a signal to move to another floor, the lift doors are automatically closed.



<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.png "image_tooltip")


Figure 5: Lift in AGV mode at floor 6 with its doors kept open.

The lift is connected directly to the building’s fire alarm signal and alerts PLC consequently. Lift immediately exits AGV mode (PLC goes into error state) and proceeds to the first floor and keeps its doors open. The alarm beacon on top of lift controller box (refer to figure 6) is enabled which lights up as a red light to alert the nearby patrons and library staff of the emergency situation.  

If the PLC is in error state, all requests are ignored until the recovery behaviour has been implemented. Section 4 explores all the possible exception scenarios in more detail.

Please note that as per project requirements, floor B1 is currently excluded from the operational flow and lift controller box cannot command the lift to reach it. This can be easily modified later if needed by making corresponding changes in the lift controller box hardware and software.


## 2.3 Lift Controller Box



<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image5.png "image_tooltip")


Figure 6: Lift Controller Box with respect to the physical system architecture.

Lift controller box is installed at the sixth floor in the service lift 4 lobby of the library. It looks as shown in figure 6, with different external components labelled in the image. A conceptual diagram with the box’s contents and its connections to external peripherals and devices is shown in figure 7.

A turnkey slot is provided to facilitate the library staff to disable or enable the AGV mode option. If it is toggled towards the Manual mode option, lift will remain in manual mode and all RMF requests to switch to AGV mode will be ignored.

A Simulated Fire Alarm button is included to mimic an actual fire alarm. Its main purpose is to help in the testing of fire alarm exception case and serves no purpose after deployment. In both simulated and actual fire, the alarm sound beacon sounds and lights up to alert the nearby patrons and staff of the emergency.



<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image6.png "image_tooltip")


Figure 7: Labelled lift controller box.



<p id="gdcalert7" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image7.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert8">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image7.png "image_tooltip")


Figure 8: Conceptual connections diagram of the lift controller box



3. Integration with Doors


## 3.1 Flow of Operations

The standard flow of operations is quite straightforward (with respect to the door controller): if the door controller is not offline, either open/close command is received and the door is respectively actuated. In open command, the door is kept open until explicitly commanded otherwise. If the door controller is offline, all requests are ignored until it is back online.


## 3.2 Door Controller Box



<p id="gdcalert8" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image8.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert9">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image8.png "image_tooltip")


Figure 9: Door Controller Boxes with respect to the physical system architecture.

Each door has its own door controller box, so there are 5 such boxes installed on levels 2 through 6. It looks as shown in figure 10, with different external components labelled in the image. A conceptual diagram with the box’s contents and its connections to external peripherals and devices is shown in figure 11.



<p id="gdcalert9" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image9.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert10">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image9.png "image_tooltip")


Figure 10: Labelled door controller box.



<p id="gdcalert10" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image10.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert11">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image10.png "image_tooltip")


Figure 11: Conceptual connections diagram of the door controller box

The limit switches are used as feedback to confirm if the door has ‘fully closed’. The pre-installed automated door controller’s internal circuit is utilised to confirm if the door has ‘fully opened’.



4. Server



<p id="gdcalert11" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image11.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert12">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image11.png "image_tooltip")


Figure 12: Server with respect to the physical system architecture.

The application is deployed via a Docker container. It utilises AWS IOT Core cloud service to connect to the RMF lift adapter. ROS2 messages are published via MQTT on specific published and subscribed AWS topics.



<p id="gdcalert12" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image12.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert13">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image12.png "image_tooltip")


Figure 13: Software Architecture of server application.

 \


A LiftState.msg is published to the AWS topic every 2 seconds which is read by RMF to get all relevant information about the lift state at that instance. The contents of the message is shown in figure 14. It contains time of publication, name of lift, available floors (floors which lift can transit to), lift’s current floor (shows actual value only in AGV mode), lift’s destination floor (this target floor was the previous destination set by RMF), lift door’s state (whether it is open, closed or moving), lift’s motion status  (moving up, moving down, stopped or unknown), lift’s current mode (offline, manual, AGV, fire or unknown) and session ID (identification details of the current AGV mode session). 



<p id="gdcalert13" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image13.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert14">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image13.png "image_tooltip")


Figure 14: ListState.msg contents.

The data to be published is retrieved via the PLC using ADS (Automation Device Specification) communication protocol, which is Beckhoff's preferred method. 

RMF commands the lift by publishing a LiftRequest.msg every 1 second on a different AWS topic which the server subscribes to. The contents of the message is shown in figure 9. It contains the time of publication, name of lift, session ID (unique for each requester, in case multiple agents need to command the lift), request type (whether to request to AGV mode, manual mode or end the current session), destination floor (relevant in AGV mode) and lift door state (whether it is open, closed or moving).



<p id="gdcalert14" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image14.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert15">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image14.png "image_tooltip")


Figure 15: LiftRequest.msg contents.

A DoorState.msg for each door is published to the respective AWS topic every 2 seconds which is read by RMF to get all relevant information about the door state at that instance. The contents of the message is shown in figure 16. It contains time of publication, name of door, and the door mode (open, closed or moving).



<p id="gdcalert15" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image15.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert16">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image15.png "image_tooltip")


Figure 16: DoorState.msg contents.

RMF commands each door by publishing a DoorRequest.msg every 1 second on an AWS topic which the server subscribes to. This topic is the same for all doors. The contents of the message is shown in figure 17. It contains the time of publication, requester ID (unique for each requester, in case multiple agents need to command the lift), name of the door (to specify which door is to be actuated), and requested mode (whether to request to open or close).



<p id="gdcalert16" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image16.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert17">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image16.png "image_tooltip")


Figure 17: DoorRequest.msg contents.



5. Exception Scenarios

The following subsections explore all possible exception cases for lift and doors and their respective recovery behaviours. The application is built in a non-blocking way, for example, if one door has error or is offline, other doors and lift are unaffected and vice-versa.


## 5.1 Lift Exceptions and Recovery Behaviour

Table 1 summarises the exception scenarios possible with regards to the lift and its controller box. It also specifies the corresponding recovery behaviour to handle the occurred exception.


    Table 1: Exception Scenarios for service lift:


<table>
  <tr>
   <td><strong>Sr. No.</strong>
   </td>
   <td><strong>Error/Exception</strong>
   </td>
   <td><strong>Action</strong>
   </td>
   <td><strong>Recovery behaviour</strong>
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>Loss of communications between server and PLC.
   </td>
   <td>
<ol>

<li>Lift goes back to manual mode.

<li>PLC state goes to error state.
</li>
</ol>
   </td>
   <td>When the heartbeat is heard again, it goes back to human mode. 
<p>
No need for human intervention.
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>Fire Alarm is activated.
   </td>
   <td>
<ol>

<li>Lift goes back to manual mode.

<li>PLC state goes to error state.

<li>Sound beacon is switched on.
</li>
</ol>
   </td>
   <td>Manual intervention is needed. PLC will stay in error state until it is restarted.
   </td>
  </tr>
  <tr>
   <td>3
   </td>
   <td>Lift is not in operational state.
   </td>
   <td>
<ol>

<li>Lift goes back to manual mode.

<li>PLC state goes to error state.
</li>
</ol>
   </td>
   <td>No need for human intervention. Operations will continue as they were after lift is back in operational state.
   </td>
  </tr>
  <tr>
   <td>4
   </td>
   <td>Simulated Fire Alarm button is pressed.
   </td>
   <td>
<ol>

<li>Lift goes back to manual mode.

<li>PLC state goes to error state.

<li>Alarm beacon is switched on.
</li>
</ol>
   </td>
   <td>Manual intervention is needed. PLC will stay in error state until it is restarted.
   </td>
  </tr>
  <tr>
   <td>5
   </td>
   <td>If turnkey is toggled towards Manual
   </td>
   <td>
<ol>

<li>Lift goes back to manual mode.

<li>PLC state goes to error state 
</li>
</ol>
   </td>
   <td>To return things to normal, toggle turnkey towards AGV mode.
   </td>
  </tr>
  <tr>
   <td>6
   </td>
   <td>Mode Timeout (10 minutes)
<p>
(Lift is unable to go to AGV mode from human mode)
   </td>
   <td>
<ol>

<li>Lift goes back to manual mode.

<li>PLC state goes to error state.
</li>
</ol>
   </td>
   <td>Manual intervention is needed. PLC will stay in error state until it is restarted.
   </td>
  </tr>
  <tr>
   <td>7
   </td>
   <td>Destination floor requested is not in available floors
   </td>
   <td>
<ol>

<li>Lift goes back to manual mode.
</li>
</ol>
   </td>
   <td>Operations flow is not hampered. RMF to send a correct destination floor in the next request.
   </td>
  </tr>
  <tr>
   <td>8
   </td>
   <td>Lift movement timeout - Lift did not reach destination floor after 2 minutes
   </td>
   <td>
<ol>

<li>Lift goes back to manual mode.

<li>PLC state goes to error state.
</li>
</ol>
   </td>
   <td>Manual intervention is needed. PLC will stay in error state until it is restarted.
   </td>
  </tr>
</table>


If PLC goes into error state, it is indicated by the last LED on its output terminal, as shown in figure 18:



<p id="gdcalert17" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image17.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert18">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image17.png "image_tooltip")


Figure 18: Error state indication by LED on PLC, encircled by red ellipse.


## 5.2 Door Exceptions and Recovery Behaviour

Table 2 summarises the exception scenarios possible with regards to the lift and its controller box. It also specifies the corresponding recovery behaviour to handle the occurred exception.


    Table 2: Exception Scenarios for doors:


<table>
  <tr>
   <td><strong>Sr. No.</strong>
   </td>
   <td><strong>Error/Exception</strong>
   </td>
   <td><strong>Action</strong>
   </td>
   <td><strong>Recovery behaviour</strong>
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>Loss of communications between server and door controller.
   </td>
   <td>
<ol>

<li>Door mode is reflected as ‘offline’.
</li>
</ol>
   </td>
   <td>Need for human intervention if it stays disconnected for a long time. After reconnection, the operation continues as before.
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>Cannot detect fully open or fully closed door state.
   </td>
   <td>
    -
   </td>
   <td>Need for manual intervention to fix the relevant sensor.
   </td>
  </tr>
</table>

