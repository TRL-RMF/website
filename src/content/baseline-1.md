# Baseline 1

## INTRODUCTION
In this baseline, we will be conducting unit testing for each of the following components:
- RMF (simulation)
- Aurorabot Fleet Adapter (Physical)
- CAATO Fleet Adapter (Physical)
- Fujitec Lift Controller and Adapter (Physical)
- Dormakaba Door Controller and Adapter (Physical)

For the purpose of this test, we will be using simulation and physical testing of each components ability to meet a set of test requirements before we proceed to baseline 2 testing.W
## OBJECTIVES AND TASKS
### Objectives
The objective of this testing is to ensure that we are able to identify potential issues as well as seek recommended solutions for these problems. We are also expecting to 'tune' the system to achieve a certain level of robustness in handling certain situations, such as receiving changing itineraries or handling conflicts between two robots.
### Tasks
In general, the tasks will occur in the following sequence:
1. Identify Test Case
2. Conduct Test with system as-is
3. Identify any issues (if any)
4. If any issues, seek remedy and modify system
5. Repeat Steps 2 - 4 until system is able to pass test case identified.
## SCOPE
### RMF Unit Testing
For the purpose of RMF unit testing, we will test using the RMF Gazebo Simulation in ROS2 using the plugin adapters. We will test 4 test cases listed below:
#### RMF Unit Test Case 1
Using the map below, and two tinyRobot plugins, we will test conflicting paths. In this case, the two robots will be sent to perform a loop task from their respective charging points to opposing points (tinyRobot1 goes to cafe_4 and tinyRobot2 goes to big_screen). The detailed test plan is in the TESTING STRATEGY section.
#### RMF Unit Test Case 2
Using the map below, and two tinyRobot plugins, we will test conflicting paths. In this case, the two robots will be sent to perform a loop task from their respective charging points to the same point via the door at lift_landing. The detailed test plan is in the TESTING STRATEGY section.
#### RMF Unit Test Case 3
Using the TRL map, and two tinyRobot plugins, we will test lift coordination. In this case, the two robots will be sent to perform a loop task from their respective charging points to different point via the lift at lift_landing. The detailed test plan is in the TESTING STRATEGY section.
#### RMF Unit Test Case 4
Using the TRL map, and two tinyRobot plugins, we will test fire alarm coordination. In this case, the two robots will be sent to perform a loop task from their respective charging points to different points via the lift at lift_landing. The detailed test plan is in the TESTING STRATEGY section.

### Aurorabot and CAATO Fleet Adapter Unit Testing 
This is the testing scope

### Door Controller and Adapter at TRL Unit Testing
This is the testing scope
### Lift Controller and Adapter at TRL Unit Testing
This is the testing scope

## TESTING STRATEGY
### RMF Unit Testing
1.  Unit Test Case 1 (Robot Deconfliction)

| Stage | Description | Expected Behaviour |
| --- | --- | --- |
| 0 | tinyRobot 1 and 2 at their respective charger points (tinyRobot1_charger and tinyRobot2_charger). | Robots are waiting at their respective charging points |
| 1 | tinyRobot_1 issued a patrol task to proceed to cafe_4 and then return to tinyRobot1_charger | tinyRobot fleet adapter submits bid, wins bid and then issues task to navigate to the stated waypoints. tinyRobot_1 receives navigation requests and proceeds to navigate |
| 2 | Delay 1 second |  |
| 3 | tinyRobot_2 issued a patrol task to proceed to big_screen and then return to tinyRobot2_charger | tinyRobot fleet adapter submits bid, wins bid and then issues task to navigate to the stated waypoints. tinyRobot fleet adapter deconflicts between the two current robots moving and tinyRobot_2 receives navigation requests and proceeds to navigate  without conflicting with tinyRobot1 |
| 4 | Both tinyRobots return to their respective charger | No conflict occurring between robots, or conflicts able to resolve and robots able to reach charging points |

2. Unit Test Case 2 (Door - Robot  Deconfliction)

| Stage | Description | Expected Behaviour |
| --- | --- | --- |
| 0 | tinyRobot 1 and 2 at their respective charger points (tinyRobot1_charger and tinyRobot2_charger). | Robots are waiting at their respective charging points |
| 1 | tinyRobot_1 issued a patrol task to proceed to lift_waiting_point and then return to tinyRobot1_charger | tinyRobot fleet adapter submits bid, wins bid and then issues task to navigate to the stated waypoints. tinyRobot_1 receives navigation requests and proceeds to navigate |
| 2 | Delay 1 second |  |
| 3 | tinyRobot_2 issued a patrol task to proceed to lift_waiting_point and then return to tinyRobot2_charger | tinyRobot fleet adapter submits bid, wins bid and then issues task to navigate to the stated waypoints. tinyRobot fleet adapter deconflicts between the two current robots moving and tinyRobot_2 receives navigation requests and proceeds to navigate  without conflicting with tinyRobot1 |
| 4 | Both tinyRobots return to their respective charger | No conflict occurring between robots, or conflicts able to resolve and robots able to reach charging points |

3. Unit Test Case 3 (Lift - Robot  Deconfliction)

| Stage | Description | Expected Behaviour |
| --- | --- | --- |
| 0 | tinyRobot 1 and 2 at their respective charger points (tinyRobot1_charger on L2 and tinyRobot2_charger on L3). | Robots are waiting at their respective charging points |
| 1 | tinyRobot_1 issued a patrol task to proceed to L3 and then return to tinyRobot1_charger | tinyRobot fleet adapter submits bid, wins bid and then issues task to navigate to the stated waypoints. tinyRobot_1 receives navigation requests and proceeds to navigate |
| 2 | Delay 1 second |  |
| 3 | tinyRobot_2 issued a patrol task to proceed to L4 and then return to tinyRobot2_charger | tinyRobot fleet adapter submits bid, wins bid and then issues task to navigate to the stated waypoints. tinyRobot fleet adapter deconflicts between the two current robots moving and tinyRobot_2 receives navigation requests and proceeds to navigate  without conflicting with tinyRobot1 |
| 4 | Both tinyRobots return to their respective charger | No conflict occurring between robots, or conflicts able to resolve and robots able to reach charging points |

3. Unit Test Case 4 (Fire Alarm)

| Stage | Description | Expected Behaviour |
| --- | --- | --- |
| 0 | tinyRobot 1 and 2 at their respective charger points (tinyRobot1_charger on L2 and tinyRobot2_charger on L3). | Robots are waiting at their respective charging points |
| 1 | tinyRobot_1 issued a patrol task to proceed to L3 and then return to tinyRobot1_charger | tinyRobot fleet adapter submits bid, wins bid and then issues task to navigate to the stated waypoints. tinyRobot_1 receives navigation requests and proceeds to navigate |
| 2 | Delay 1 second |  |
| 3 | tinyRobot_2 issued a patrol task to proceed to L4 and then return to tinyRobot2_charger | tinyRobot fleet adapter submits bid, wins bid and then issues task to navigate to the stated waypoints. tinyRobot fleet adapter deconflicts between the two current robots moving and tinyRobot_2 receives navigation requests and proceeds to navigate  without conflicting with tinyRobot1 |
| 4 | Fire alarm is triggered | Both robots stop and proceed to nearest parking spot |

### Aurorabot and CAATO Fleet Adapter Unit Testing

###  Door Controller and Adapter at TRL Unit Testing

### Lift Controller and Adapter at TRL Unit Testing

## EQUIPMENT REQUIREMENTS
### RMF Unit Testing
1. Computer running RMF on Galactic with latest release of RMF and TRL repositories

### Aurorabot and CAATO Fleet Adapter Testing
1. Aurorabot (or CAATO) robot with Client running
2. RMF Staging Server on AWS GCC Running with RMF-Web displayed

###  Door Controller and Adapter at TRL Unit Testing

### Lift Controller and Adapter at TRL Unit Testing

## TEST SCHEDULE

## CONTROL PROCEDURES

## FEATURES TO BE TESTED

## FEATURES NOT TO BE TESTED
 RESOURCES/ROLES & RESPONSIBILITIES
## RESOURCES/ROLES & RESPONSIBILITIES

## SCHEDULES

## RISKS/ASSUMPTIONS

![](https://rmf-systems-engineering-handbook.s3.ap-southeast-1.amazonaws.com/baseline-1/MBC_sim_1_lane_deconflict.gif)

![](https://rmf-systems-engineering-handbook.s3.ap-southeast-1.amazonaws.com/baseline-1/MBC_sim_1_lane_with_big_screen_holding_point.gif)

![](https://rmf-systems-engineering-handbook.s3.ap-southeast-1.amazonaws.com/baseline-1/MBC_sim_1_lane_with_passthrough_points_2.gif)

![](https://rmf-systems-engineering-handbook.s3.ap-southeast-1.amazonaws.com/baseline-1/MBC_sim_1_lane_with_passthrough_points_3.gif)

![](https://rmf-systems-engineering-handbook.s3.ap-southeast-1.amazonaws.com/baseline-1/MBC_sim_1_lane_with_passthrough_points_fail.gif)

![](https://rmf-systems-engineering-handbook.s3.ap-southeast-1.amazonaws.com/baseline-1/MBC_sim_1_lane_with_passthrough_points.gif)

![](https://rmf-systems-engineering-handbook.s3.ap-southeast-1.amazonaws.com/baseline-1/MBC_sim_multilane_deconflict.gif)

![](https://rmf-systems-engineering-handbook.s3.ap-southeast-1.amazonaws.com/baseline-1/MBC_sim_multulane_deconflict_2.gif)

