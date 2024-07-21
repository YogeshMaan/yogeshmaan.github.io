## To-Do's
[x] Setup the F/T sensor on Drone by [May 02, 2024]
[x] 

## Setup
My IP address: 192.168.1.3 

UR IP address: 192.168.1.9 (can be found in about page of UR5 display)



### Simulation


### Resources
* [UR-Robot-ROS-Driver](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver)
* Setup a real-time enabled linux system [refer this!](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/real_time.md) Not needed if latency is not an issue.
* https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/usage_example.md
* Tutorial https://roboticscasual.com/ros-tutorial-control-the-ur5-robot-with-ros_control-tuning-a-pid-controller/
* [Usage example](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/usage_example.md)


## Required installation on Ubuntu 20.04
- ROS-noetic
- UR5 driver for noetic (ur_robot_driver)
- 
- Connect to communication network
- ROS control 


## UR5 
- Connect ethernet 
- get pc ip address
- Installation>ProfiNet: Disable
- Installation>Ethernet/IP: Disable
- ModBus: Disable
- External Control: My Pc ip address
- Load Program>ROS control.urp file
- Start driver on ubuntu
- Press Play button on GUI to start commanding goals


```bash
roslaunch ur_robot_driver <robot_type>_bringup.launch robot_ip:=192.168.56.101if
```


####  Interface
- Enable ethernet communication with UR5 interface
- Enable  

## Demo
Once the UR5 is connected and running on ROS1 network.

The following nodes are running
```bash
/controller_stopper
/robot_state_publisher
/ros_control_controller_spawner
/ros_control_stopped_spawner
/rosout
/ur_hardware_interface
/ur_hardware_interface/ur_robot_state_helper
```

The following topics are available
```bash
/joint_group_vel_controller/command
/joint_states
/pos_joint_traj_controller/command
/pos_joint_traj_controller/follow_joint_trajectory/cancel
/pos_joint_traj_controller/follow_joint_trajectory/feedback
/pos_joint_traj_controller/follow_joint_trajectory/goal
/pos_joint_traj_controller/follow_joint_trajectory/result
/pos_joint_traj_controller/follow_joint_trajectory/status
/pos_joint_traj_controller/state
/rosout
/rosout_agg
/scaled_pos_joint_traj_controller/command
/scaled_pos_joint_traj_controller/follow_joint_trajectory/cancel
/scaled_pos_joint_traj_controller/follow_joint_trajectory/feedback
/scaled_pos_joint_traj_controller/follow_joint_trajectory/goal
/scaled_pos_joint_traj_controller/follow_joint_trajectory/result
/scaled_pos_joint_traj_controller/follow_joint_trajectory/status
/scaled_pos_joint_traj_controller/state
/speed_scaling_factor
/tf
/tf_static
/ur_hardware_interface/io_states
/ur_hardware_interface/robot_mode
/ur_hardware_interface/robot_program_running
/ur_hardware_interface/safety_mode
/ur_hardware_interface/script_command
/ur_hardware_interface/set_mode/cancel
/ur_hardware_interface/set_mode/feedback
/ur_hardware_interface/set_mode/goal
/ur_hardware_interface/set_mode/result
/ur_hardware_interface/set_mode/status
/ur_hardware_interface/tool_data
/wrench


```


## Discussions
- **[April 26, 2024]** We need to achieve velocity control for end-effector and tune the parameter 'approach_velocity' to get a desired force. 
- **[May 03, 2024]** Only work on a single joint (may be wrist) for joint_velocity-control. To repeat the impacting pattern.
- **[May 10, 2024]** Figure out the write topic to publish the end effector pose. I have tried gazebo but its seems complicated. MoveIt might work straight forward. Then, check suitable topic on real hardware as well.
- **[May 14, 2024]**  Normal consistent trajectory to hit at same point works. It's possible to include the velocity setpoint as well. However, how will be ensure the UAVs location as the UAV doesn't hover at a point precisely.
- **[May 17, 2024]** we can plan a cartesian trajectory using test_move script of ur_robot_driver pkg. Need to design a mount for ur5 and drone such that impact force can be applied.



## Debugging
1. More space needed to install real-time kernel for UR5 (~25GB)
2. sudo apt-get -- "command not found"


## RViz and Moveit
```bash
roslaunch ur5_moveit_config demo.launch
```

## Gazebo
### Resources
- Issue mentioned on UR forum [here](https://forum.universal-robots.com/t/unexpected-behavior-in-gazebo-while-crossing-180-degrees/16096/3). Helpful, for gazebo simulation uses Robotis System Toolbox from MathWorks to generate a set of joint angles. 
```bash
roslaunch ur_gazebo ur5_bringup.launch
```
**Cartesian-trajectory controller**: Unavailable 

**Joint-trajectory-planning**: Need to create a JointSpaceTrajectory() from CartesianWaypoints for the UR5 using Matlab tools. Once the joint_space_trajectory is obtained. We can publish the joint_traj to the joint_traj_controller topic. 

## Custom Node: Cartesian trajectory planning
Script: /home/yogesh/noetic_ws/src/Universal_Robots_ROS_Driver/ur_robot_driver/scripts/test_move.py

```bash
rosrun ur_robot_driver test_move #.py?
```
### Issue
1. When planning a cartesian trajectory using the available cartesian controller and test_move script, the home is unknown. How to check the current pose of end-effector in cartesian coordinates?

#### current state in cartesisan space of UR5

#### home state in cartesian space of ur5


