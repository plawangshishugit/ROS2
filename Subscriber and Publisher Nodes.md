1. Check the .bashrc file in order to make sure that the ROS environment is sourced
```
getit ~/.bashrc
```
At the end we should have: 
source /opt/ros/humble/setup.bash
If not then add this line.
2. Create a workspace
```
mkdir -p ~/testros/src
cd ~/testros
catkin_make
```
