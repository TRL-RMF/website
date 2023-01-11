# Design Documents

An excellent resource to have an overview of RMF and the integration process is the [ROS2 multirobot book](https://osrf.github.io/ros2multirobotbook/), it's recommended to read it before embarking in any integration effort.

## RMF-BMS Integrator

Integration between RMF and infrastructure (doors, lifts and in some cases workcells) is done through adapters.

The purpose of infrastructure adapters is to act as a translation layer between proprietary APIs (i.e. PLCs, cloud based) and RMF. Open source implementation are available in the [awesome_adapters](https://github.com/open-rmf/awesome_adapters) repo, at the time of writing for [Kone lifts](https://github.com/sharp-rmf/kone_lift_controller) and [Dormakaba doors](https://github.com/open-rmf/door_adapter_dormakaba).

Templates are also provided for both [doors](https://github.com/open-rmf/door_adapter_template) and [https://github.com/open-rmf/lift_adapter_template](https://github.com/open-rmf/lift_adapter_template). In templates, integrators only need to fill a few functions that are specific to the API of the chosen device, for example for doors, a function to open, one to close and one to query the state of the door.

Once a door / lift adapter is added to the system RMF, and through it any fleet that is integrated in the future, will be able to use it for operations.

## RMF-Core (Cloud) Integrator

A [template](https://github.com/open-rmf/rmf_deployment_template/) is provided to integrators to help with cloud deployment, it is rapidly growing so it is recommended to check the repository itself for the latest integration instructions.

At the date of writing, it uses github actions to build docker images which are hosted in a cloud registry and ran in a cloud kubernetes (k3s) cluster configured through helm charts. The template itself only builds the core RMF services but provides examples on how to add custom pods (i.e. to run fleet adapters or infrastructure adapters).

## RMF-AMR Fleet Integrator

Integration between RMF and robot fleets is done through fleet adapters.

Similar to infrastructure adapters, several open source implementations for a variety of robot vendors / models are available in the [awesome_adapters](https://github.com/open-rmf/awesome_adapters) repo. We also provide both a [fleet adapter template](https://github.com/open-rmf/fleet_adapter_template/) and a [demo fleet adapter](https://github.com/open-rmf/rmf_demos/tree/main/rmf_demos_fleet_adapter) that works with Gazebo simulations, depending on preference either can be used to integrate with a new robot vendor.

Several types of integration are possible depending on the level of control given by the robot vendor, for details on the process consult the relevant [multirobot book chapter](https://osrf.github.io/ros2multirobotbook/integration_fleets.html)


## Overall Systems Integrator

An overview of the overall system integration effort is available in the [Integration chapter](https://osrf.github.io/ros2multirobotbook/integration.html) in the multirobot book. As an overview of the steps that are part of a usual deployment:

1. Obtain building blueprints / maps, then design a traffic map through [traffic editor.](https://github.com/open-rmf/rmf_traffic_editor)
2. Integrate and configure the desired mobile robot fleets through fleet adapters.
3. Integrate as needed doors, lifts or workcells through infrastructure adapters.
4. Test on a local machine for simplicity to make sure all the parts work as needed.
5. Create a copy of the [https://github.com/open-rmf/rmf_deployment_template/](https://github.com/open-rmf/rmf_deployment_template/) and setup the image building / cloud cluster pipeline.
6. Test and iterate, updating cloud images when needed, until performance is satisfactory.
