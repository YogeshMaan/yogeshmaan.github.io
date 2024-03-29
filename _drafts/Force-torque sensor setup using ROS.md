This document is a how to setup guide for Bota Systems- Force-Torque Rokubi serial using ROS

[Link](https://www.botasys.com/force-torque-sensors/rokubi) to official datasheet

## Requirements
1. ROS-Noetic or ROS2-Foxy
2. BotaSys FT sensor 

## Hardware description

- **Product**: Bota systems-BFT-ROKS-SER-M8
- **Range(Fxy, Fz, Mxy, Mz)**: (500N, 1200N, 15Nm, 12Nm)
- **Weight**: ~113g
- **Comunication**: Serial RS422/USB
- **Size(DxL)**: 42 x 32 mm 
- **Accuracy**: <2.0%
- **Sampling rate(Max)**: 800 Hz


## Setup 

### Install dependencies


### Wiring connections

### Data logging

### Verify 
Run (for ROS-noetic)
`rostopic list` and echo the topic of interest `rostopic echo <your topic name>`

# Design of Experiments
**Objective**: Collect UAV's trajectory data post an applied impulse force input.

### To-Do
- [ ] **[Week - March 27 - March 31]** Trajectory for end-effector(UR5)- Setpoint($x_{sp}, v_{sp}, a_{sp}$)
- [ ] How to mount FT sensor to validate with drone?
- [ ]  Plan trajectory using UR5 - Yogesh
- [ ]  Sensor calibration and integration- Karishma

### Discussions
**DATE: March 27, 2024**

 We'll be using UR5 to exert the force on UAV. The applied force on UAV will be analysed using both- the FT sensor, and the onboard external wrench estimator. How to mount sensor on UAV? Tethered wire to UAV and ROS-noetic, however, if Karishma figures out ROS2-Foxy based driver code, we'll use ROS2 Foxy. 
 How to plan the trajectory using UR5? Figure out the ROS version used by UR5 and the topics involved in communication. Then, write ROS node to execute the given trajectory to UR5.
 
 **Note**: the UR5 needs to detect the collision and return back to "homing location" post collision.
 
 




