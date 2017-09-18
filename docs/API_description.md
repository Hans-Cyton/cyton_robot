### API 简介
if you don't speak chinese, please [click here](./API_description_english.md).
#### Subscribed Topics:

**claw_controller/command (std_msgs/Float64)**
控制夹爪电机的转动，取值范围：0（打开）～-2.0（闭合）

**cyton_robot/command/joint (sensor_msgs/JointState)**
控制机械臂到达指定臂型的指令
example: /cyton_bringup/script/cmd_pub.py

**cyton_robot/command/pose (geometry_msgs/PoseStamped)**
控制机械臂末端到达指定空间位置的指令
example: /cyton_bringup/script/cmd_pub.py

**cyton_arm_controller/follow_joint_trajectory/cancel (actionlib_msgs/GoalID)**
发布空的消息可以停止正在执行中的轨迹运动
example:
```sh
$ rostopic pub /cyton_arm_controller/follow_joint_trajectory/cancel actionlib_msgs/GoalID -- {}
```
*注：以下Subscribed Topics在仿真环境下是没有的。*

**cyton_command_joint (cyton_msg/Float64Array)**
控制机械臂各个关节位置的指令
example:
```sh
$ rostopic pub /cyton_command_joint cyton_msgs/Float64Array "data: [0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, -0.2]"
```

#### Published Topics:

**joint_states (sensor_msgs/JointState)**
各个关节的当前状态

**tf (tf2_msgs/TFMessage)**
反映各轴间坐标转换关系

#### Services:
*注：以下Service在仿真环境下是没有的。*

**cyton_go_home (std_srvs/SetBool)**
让机械臂回到初始位姿
example:
```sh
$ rosservice call /cyton_go_home "data: false"
or
$ rosservice call /cyton_go_home "data: true"
```

**cyton_torque_enable (std_srvs/SetBool)**
设置机械臂使能状态
example:
使能：
```sh
rosservice call /cyton_torque_enable "data: true" 
```
去使能：
```sh
rosservice call /cyton_torque_enable "data: false" 
```
