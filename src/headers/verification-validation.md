# Verification and Validation - RMF Scheduler

Verification and validation was done in two steps in simulated environments in Gazebo.

## Rationale

Having Software-in-the-loop (SIL) testing using simulation allows us to identify bugs and errors in RMF without the need for physical testing. The early detection of problems allows for quick fixes and significantly reduces the development time and cost.

For this test, we are not simulating the entirety of the robot and building. Instead, we are focusing mainly on testing the RMF Scheduler, the backbone to RMF's operations.

## Objectives

We aim to achieve the following:

- Verify the capabilities of the RMF Scheduler
- Identify the breaking points of the RMF Scheduler


## Methodology

To achieve our objectives, we created 2 groups of test scenarios - basic tests and standalone tests.

**Basic tests** refer to the core competencies of the RMF scheduler, such as ability to interface with doors, lifts, as well as perform basic traffic conflict resolution in the deployment world. The testing information and results are available [here](../content/basic-tests.md).

**Standalone tests** refer to the limits and breaking points of the RMF scheduler, it used a world designed to create a scenario with a high amount of traffic conflicts, that the scheduler has to constantly solve to avoid deadlocks. The testing information and results are available [here](../content/standalone-tests.md).
