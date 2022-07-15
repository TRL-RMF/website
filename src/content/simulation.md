# Gazebo SIL Simulation

## Rationale
Having Software-in-the-loop (SIL) testing using simulation allows us to identify bugs and errors in RMF without the need for physical testing. The early detection of problems allows for quick fixes and significantly reduces the development time and cost.

For this project, we are not simulating the entirety of the robot and building. Instead, we are focusing mainly on testing the RMF Scheduler, the backbone to RMF's operations.

## Objectives
We aim to achieve the following:
- Verify the capabilities of the RMF Scheduler 
- Identify the breaking points of the RMF Scheduler


## Methodology
To achieve our objectives, we created 2 groups of test scenarios - basic tests and standalone tests.

Basic tests refer to the core competencies of the RMF scheduler. The testing information and results are available [here](basic-tests.md).

Standalone tests refer to the limits and breaking points of the RMF scheduler. The testing information and results are available [here](standalone-tests.md).

The documentation is a breakdown of what was presented in [this slideshow](https://docs.google.com/presentation/d/13jBM4jF8lTpDRRpXdSQ906ZPIeMKCdShkHm6D-OAvRY/edit?usp=sharing).
