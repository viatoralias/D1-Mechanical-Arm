# SDK and sample program running environment code

## 1. Install Unitree D1-T ROS Packages

Clone and build Unitree’s official ROS packages:
```bash
cd ~/catkin_ws/src
git clone https://github.com/unitreerobotics/unitree_ros.git
cd ..
catkin_make
source devel/setup.bash
```
## 2. Configure Unitree D1-T Communication

1. **Hardware setup**:
- Connect the D1-T to your computer via Ethernet (preferred for WSL).
- Assign a static IP to your WSL network adapter to match the D1-T’s default IP (192.168.123.100).
2. **Update ROS Launch Files**:
- Edit the D1-T’s launch file to match your robot’s IP:
```bash
nano ~/catkin_ws/src/unitree_ros/unitree_d1t_bringup/launch/robot_arm.launch`
```
- Look for <param name="robot_ip" value="192.168.123.161"/> and confirm it matches your robot’s IP.

## 3. Launch the D1-T ROS Driver

- Start the driver node to enable communication:
```bash
roslaunch unitree_d1t_bringup robot_arm.launch
```
- Check active topics with `rostopic list`. You should see `/joint_states` and `/arm_commands`.

## 4. Enable Motor Control

The D1-T may require explicit motor enable commands. Use ROS services:
```bash
# Check for motor enable services
rosservice list | grep enable

# Example service call (if the service exists)
rosservice call /unitree_d1t/enable_motors "data: true"
```

## 5. Test Basic Motion

Run a simple Python script to send joint commands:
```py
#!/usr/bin/env python3
import rospy
from sensor_msgs.msg import JointState

rospy.init_node('d1t_test')
pub = rospy.Publisher('/arm_commands', JointState, queue_size=10)

joint_cmd = JointState()
joint_cmd.position = [0.0, 0.5, 0.0, 0.0, 0.0, 0.0]  # Adjust for your D1-T

rate = rospy.Rate(10)
while not rospy.is_shutdown():
    pub.publish(joint_cmd)
    rate.sleep()
```