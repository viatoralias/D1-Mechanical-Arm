# Environment Setup

## 1. Installing Ubuntu

> In our case, we only used the terminal version of Ubuntu, if you prefer to use Dual-Boot Ubuntu, please refer to [here](https://releases.ubuntu.com/20.04.6/?_gl=1*19ip6hm*_gcl_au*MTE4NTIyOTI0MS4xNzA3MTMxMDQx&_ga=2.149898549.2084151835.1707729318-1126754318.1683186906).
1. Open PowerShell or any other command terminal as Administrator.
2. Install _WSL_ using the command `wsl --install`, then restart your computer.
3. Install Ubuntu 20.04 LTS from the Microsoft Store.

## 2. Installing ROS

1. Since we're using Ubuntu 20.04, install ROS Noetic, the official ROS1 distribution for this OS:
```bash
# Configure the ROS repository
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
sudo apt update

# Install ROS Noetic Desktop-Full (recommended for GUI tools)
sudo apt install ros-noetic-desktop-full

# Initialize rosdep (dependency management)
sudo rosdep init
rosdep update

# Add ROS to your shell environment
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

2. We create a ROS workspace to build our project:
```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin_make
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

3. We install essential ROS packages for robotic arm control:
```bash
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
sudo apt install ros-noetic-moveit ros-noetic-gazebo-ros-pkgs ros-noetic-joint-state-publisher
```

4. Since we're using WSL, we configure networking to communicate with hardware (critical for real robots):
```bash
# Set ROS to use your WSL IP (replace <YOUR_WSL_IP> with your actual IP)
echo "export ROS_MASTER_URI=http://<YOUR_WSL_IP>:11311" >> ~/.bashrc
echo "export ROS_IP=<YOUR_WSL_IP>" >> ~/.bashrc
source ~/.bashrc
```

5. We test our ROS installation:
```bash
# Terminal 1: Start ROS core
roscore

# Terminal 2: Run a simple ROS node
rosrun rospy_tutorials talker

# Terminal 3: Listen to the topic
rosrun rospy_tutorials listener
```  
Seeing a message means ROS is working.