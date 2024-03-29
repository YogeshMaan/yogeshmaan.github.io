This document is a how to setup guide for Bota Systems- Force-Torque Rokubi serial using ROS-Noetic

[Link](https://www.botasys.com/force-torque-sensors/rokubi) to official datasheet

Source Code from Bota Systems: [here](https://gitlab.com/botasys)

## Dependencies
1. Installed ROS-Noetic 
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
`roslaunch rokumini_serial sample.launch` 

## FAQ 

**Do I need soem and ethercat_grant installed for serial?**

The packages gets installed automatically with rosdep. 

Although, the Botasys soem documentation on Github mentions no support for noetic, support packages exist!









