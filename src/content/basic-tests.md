# Basic Test Videos
1. [Introduction](#introduction)
   1. [Basic Test 1](#basic1)
   2. [Basic Test 2](#basic2)
   3. [Basic Test 3](#basic3)
   4. [Basic Test 4](#basic4)
2. [Summary of Test Results](#summary)
## Introduction <a name="introduction"></a>
Below are a series of tests that verify the capabilities of the RMF scheduler. These tests are being conducted in simulation, using the Tampines Regional Library (TRL) world that was generated using OSRC's Traffic Editor. The relevant files can be found in [this repository](https://github.com/TRL-RMF/trl). 
The results are shown in the videos under each test statement.
## Basic Test 1 : Moving through lifts and doors <a name="basic1"></a>
The robot is expected to move through lift(s) and door(s) to navigate to its eventual destination. 

<video src="https://user-images.githubusercontent.com/54682777/178696368-53193b9f-be59-4b3d-bba7-eac2c2be3fb2.mp4" controls="controls" style="max-width: 730px;">
</video>

## Basic Test 2 : Deconflict-ing robots through lift <a name="basic2"></a>
RMF is expected to demonstrate ability to de-conflict navigation plans of 2 robots from different OEMs, both of which are using the lift. 

<video src="https://user-images.githubusercontent.com/54682777/178697123-48d2bcea-61a3-44c0-8045-ca93de4ace4c.mp4" 
controls="controls" style="max-width: 730px;">
</video>

## Basic Test 3 : Deconflict-ing robots through door <a name="basic3"></a>
RMF is expected to demonstrate ability to de-conflict navigation plans of 2 robots from different OEMs, both of which are using the same door.

<video src="https://user-images.githubusercontent.com/54682777/178697144-3eb1b4d6-607d-4d67-bc06-5dea07b3410f.mp4" controls="controls" style="max-width: 730px;">
</video>

## Basic Test 4 : Emergency Alarm <a name="basic4"></a>
RMF is expected to demonstrate ability to navigate to a safe “parking spot” when the emergency alarm is activated.

<video src="https://user-images.githubusercontent.com/54682777/178697161-52193789-93af-479e-b646-15fd760114ae.mp4" controls="controls" style="max-width: 730px;">
</video>

# Summary of Tests & Results <a name="summary"></a>

|              | Integration (Doors) | Integration (Lifts) | Traffic Deconflicting | Emergency Alarm | Verified? |
|:--------------:|:---------------------:|:---------------------:|:-----------------------:|:-----------------:|:-----------:|
| **Basic Test 1** |✔️|✔️|||✅|
| **Basic Test 2** ||✔️|✔️||✅|
| **Basic Test 3** |✔️||✔️||✅|
| **Basic Test 4** ||||✔️|     :warning:     |

As shown in Basic Tests 1-3, it was found that the RMF Scheduler was largely able to fulfil its basic competencies of controlling the lifts and doors, along with active traffic de-conflicting.

In Basic Test 4, RMF has a limitation. When the robot moves along the expected lanes of RMF, it is able to return to the nearest "safe" waypoint during the activation of the emergency alarm. However, if the robot is midway through the bookshelf scanning task, it will not be able to go back to the nearest "safe" waypoint. A way to work around this behavior at the time of writing (Dec 2022) is to manually cancel the task the robot is doing, after the task is cancelled the robot will be routed to the closest parking spot.