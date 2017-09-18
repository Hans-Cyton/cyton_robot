Cyton Robot
======

This repository provides ROS support for the Cyton Robot. The recommend operating environment is on Ubuntu 14.04 with ROS Indigo. So far These packages haven't been tested in other environment.

### Installation

First set up a catkin workspace (see [this tutorials](http://wiki.ros.org/catkin/Tutorials)).  
Then clone the repository into the src/ folder. It should look like /path/to/your/catkin_workspace/src/cyton_robot.  
Make sure to source the correct setup file according to your workspace hierarchy, then use catkin_make to compile.  

Assuming your catkin workspace folder is ~/catkin_ws, you should use the following commands:
```sh
$ cd ~/catkin_ws/src
$ git clone https://github.com/Hans-Cyton/cyton_robot.git
$ cd ..
$ catkin_make
$ source devel/setup.bash
```

Then you should install or upgrade MoveIt!. If you have installed MoveIt!, please make sure that it's been upgraded to the latest version.

**Install MoveIt!**

```sh
$ sudo apt-get install ros-indigo-moveit
$ sudo apt-get install ros-indigo-moveit-full-pr2
$ sudo apt-get install ros-indigo-moveit-kinematics
$ sudo apt-get install ros-indigo-moveit-ros-move-group
```
**Upgrade MoveIt!:**

```sh
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install ros-indigo-moveit-kinematics
$ sudo apt-get install ros-indigo-moveit-ros-move-group
```

---

### Usage with Gazebo Simulation

Bring up the simulated robot in Gazebo:
```sh
$ roslaunch cyton_model gazebo.launch
```
Start up RViz with a configuration including the MoveIt! Motion Planning plugin:
```sh
$ roslaunch cyton_moveit_config moveit_planning_execution.launch
```
> Tutorial about how to use MoveIt! RViz plugin: [docs/moveit_plugin_tutorial_english.md](docs/moveit_plugin_tutorial_english.md)  
Tips:
Every time you want to plan a trajectory, you should set the start state to current first.

For the teleoperation with keyboard, run:
```sh
$ rosrun cyton_teleop cyton_teleop_keyboard
```
For the teleoperation with joystick, run:
```sh
$ roslaunch cyton_moveit_config joystick_control.launch
```
> Tutorial about joystick control teleoperation:  
http://docs.ros.org/indigo/api/moveit_tutorials/html/doc/ros_visualization/joystick.html  
Tips:  
1. In the Motion Planning plugin of Rviz, enable “Allow External Comm.” checkbox in the “Planning” tab.  
2. Add “Pose” to rviz Displays and subscribe to /joy_pose in order to see the output from joystick. Note that only planning groups that have IK solvers for all their End Effector parent groups will work.

---

### Usage with real Hardware
Firstly, please install the driver for the servos.

Make sure that you didn't install the package dynamixel_motor. If you did, please remove them with the following command.
```sh
$ sudo apt-get remove ros-indigo-dynamixel-motor ros-indigo-dynamixel-driver ros-indigo-dynamixel-controllers ros-indigo-dynamixel-msgs ros-indigo-dynamixel-tutorials
```
Then install your driver according to the servo brand.

If you are using **dynamixel** servos, install the driver with following commands:
```sh
$ cd your_catkin_ws/src
$ git clone https://github.com/onionsflying/dynamixel_motor.git
$ cd ..
$ catkin_make
```
If you are using **Han's xQtor** servos, install the driver with following commands:
```sh
$ cd your_catkin_ws/src
$ git clone https://github.com/Hans-Cyton/dynamixel_motor.git
$ cd ..
$ catkin_make
```

Then connect Cyton to the computer with a usb cable. You can see the usb device ID(s) with the following command.
```sh
$ ls /dev/ttyUSB*
```
If there are more than one usb devices, you should confirm the ID(s) of the other device(s) before you connect Cyton to the computer. In this case you can find out the device ID of Cyton. The default ID is /dev/ttyUSB0. If your device ID is not /dev/ttyUSB0, you should correct the following line in the file cyton_bringup/launch/cyton_bringup.launch
```
port_name: "/dev/ttyUSB0"
```

Assuming the device ID is /dev/ttyUSB0, to bring up the real robot, run:
```sh
$ sudo chmod 777 /dev/ttyUSB0
$ roslaunch cyton_bringup cyton_bringup.launch
```
Make the robot go to home position:
```sh
$ rosservice call /cyton_go_home "data: true"
or
$ rosservice call /cyton_go_home "data: false"
# for this command "data: false" and "data: true" are the same
```

Start up RViz with a configuration including the MoveIt! Motion Planning plugin:
```sh
$ roslaunch cyton_moveit_config moveit_planning_execution.launch
```
> Tutorial about how to use MoveIt! RViz plugin: [docs/moveit_plugin_tutorial_english.md](docs/moveit_plugin_tutorial_english.md)  
Tips:
Every time you want to plan a trajectory, you should set the start state to current first.

For the teleoperation with keyboard, run:
```sh
$ rosrun cyton_teleop cyton_teleop_keyboard
```
For the teleoperation with joystick, run:
```sh
$ roslaunch cyton_moveit_config joystick_control.launch
```
> Tutorial about joystick control teleoperation:  
http://docs.ros.org/indigo/api/moveit_tutorials/html/doc/ros_visualization/joystick.html  
Tips:  
1. In the Motion Planning plugin of Rviz, enable “Allow External Comm.” checkbox in the “Planning” tab.  
2. Add “Pose” to rviz Displays and subscribe to /joy_pose in order to see the output from joystick. Note that only planning groups that have IK solvers for all their End Effector parent groups will work.

Before turning the robot off, you can use the following command to set torque disenable. At this time you should protect the robot from falling down.
```sh
$ rosservice call /cyton_torque_enable "data: false" 
# "data: false" : disenable
# "data: true": enable 
```
