### API description

#### Subscribed Topics:

**claw_controller/command (std_msgs/Float64)**
control the servo for the claw, values range: 0(opened)～-2.0(closed)

**cyton_robot/command/joint (sensor_msgs/JointState)**
make the robot move to a position in joint space.
example: /cyton_bringup/script/cmd_pub.py

**cyton_robot/command/pose (geometry_msgs/PoseStamped)**
make the robot move to a position in cartesian coordination system.
example: /cyton_bringup/script/cmd_pub.py

**cyton_arm_controller/follow_joint_trajectory/cancel (actionlib_msgs/GoalID)**
stop a trajectory action in executing by publishing an empty topic.
example:
```sh
$ rostopic pub /cyton_arm_controller/follow_joint_trajectory/cancel actionlib_msgs/GoalID -- {}
```
*Note: the following Subscribed Topics are invalid in simulation*

**cyton_command_joint (cyton_msg/Float64Array)**
control the positions of the robot's joints.
example:
```sh
$ rostopic pub /cyton_command_joint cyton_msgs/Float64Array "data: [0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, -0.2]"
```

#### Published Topics:

**joint_states (sensor_msgs/JointState)**
the current status of the joints.

**tf (tf2_msgs/TFMessage)**
the transformation relationship among the links.

#### Services:
*Note: the following Services are invalid in simulation*

**cyton_go_home (std_srvs/SetBool)**
make the robot go to home position.

example:
```sh
$ rosservice call /cyton_go_home "data: false"
or
$ rosservice call /cyton_go_home "data: true"
```

**cyton_torque_enable (std_srvs/SetBool)**
set torque enable/disenable

example:

enable:
```sh
rosservice call /cyton_torque_enable "data: true" 
```
disenable:
```sh
rosservice call /cyton_torque_enable "data: false" 
```
