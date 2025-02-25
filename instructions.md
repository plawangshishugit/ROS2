## **1. Running Executables**  

### **Install Turtlesim**  
To install `turtlesim`, run the following commands:
```bash
sudo apt update
sudo apt install ros-humble-turtlesim
```

### **See List of Packages**  
To list all available executables:
```bash
ros2 pkg executables
```

### **See List of Executables from Turtlesim Package**  
To check the executables available in the `turtlesim` package:
```bash
ros2 pkg executables turtlesim
```

### **Find Turtlesim Location**  
Navigate to the ROS installation directory:
```bash
cd /opt/ros/humble/
code .
```
- **`Ctrl + P`** → Search for **`turtlesim`**  
- You should find **turtlesim** located in:  
  ```bash
  share/ament_index/resource_index/packages
  ```

### **Run Turtlesim**  
Use the following command to start the `turtlesim` node:
```bash
ros2 run turtlesim turtlesim_node
```
> **Tip:** Use `ros2 run <package_name> <executable_name> -h` to see available options.

### **Run Teleop Mode in Another Terminal**  
To control the turtle interactively, run:
```bash
ros2 run turtlesim turtle_teleop_key
```

## **2. RQT**  

### **Overview**  
"ROS Qt-based framework" is a Qt-based graphical user interface (GUI) framework used for visualizing and debugging ROS nodes, topics, and messages. It provides various plugins for plotting data, managing parameters, and monitoring system performance.

### **Install RQT**  
To install `rqt`, run the following command:
```bash
sudo apt install ~nros-humble-rqt*
```

### **Run RQT**  
To launch the `rqt` GUI:
```bash
rqt
```
- This will open a GUI. Navigate to:
  - **Plugins** → **Service** → **Service Caller**
  - Select **Spawn** in the service menu.

### **Run Turtlesim**  
Start the `turtlesim` node:
```bash
ros2 run turtlesim turtlesim_node
```

### **Run Teleop Mode in Another Terminal**  
To enable keyboard control:
```bash
ros2 run turtlesim turtle_teleop_key
```

### **Change Control to a New Turtle**  
To remap control to a new turtle:
```bash
ros2 run turtlesim turtle_teleop_key --ros-args --remap turtle/cmd_vel:=turtle2/cmd_vel
```
----------------------------------------------------------------------------------------

# 3. ROS Topic
A topic is one way data is moved between nodes. where 1 or more publisher nodes can connect to 1 or more subscriber nodes.

1. open new terminal and run

```
ros2 run <package_name> <executable_node>
ros2 run turtlesim turtlesim_node 
```
2. in another terminal, start the teleop node
```
ros2 run turtlesim turtle_teleop_key
```
3. see list of topics
```
ros2 topic list
see details
ros2 topic list -t
```
4. start qrt graph. Unlock hide boxes to see hidden topics.
```
rqt_graph
```
5. see topic output. Move with teleop to see output after running command
```
ros2 topic echo <topic_name>

ros2 topic echo /turtle1/cmd_vel
```
6. View topic info
```
ros2 topic info <topic_name>

ros2 topic info /turtle1/cmd_vel
```
7. See interface definition 
```
ros2 interface show <type>
ros2 interface show geometry_msgs/msg/Twist
```
8. Publish data to a topic ( `` --once `` means publish once and exit)
```
ros2 topic pub <topic_name> <msg_type> '<args>'

ros2 topic pub --once /turtle/cmd_vel geometry msgs/msg/Twist "{linear:{x:2.0,y:0.0,z:0.0},angular:{x:0.0,y:0.0,z:1.8}}"
```
9. Publish data to a topic with rate 1 hz
```
ros2 topic pub --rate 1 /turtle/cmd_vel geometry msgs/msg/Twist "{linear:{x:2.0,y:0.0,z:0.0},angular:{x:0.0,y:0.0,z:1.8}}"
```
10. Echo the pose topic. Go to the rqt graph and refresh.
```
ros2 topic echo /turtle/turtle1/pose
```
11. View topic freq
```
ros2 topic hz <topic_name>

ros2 topic hz /turtle1/pose
```
----------------------------------------------------------------------------
# 4. ROS Service
Services are used to communicate between nodes using a client-server model. Where the server responds when the client make a request.

1. In a new terminal, run
```
ros2 run turtlesim turtlesim_node
```
2. In another terminal, run
```
ros2 run turtlesim turtle_teleop_key
```
3. List all service names
```
ros2 service list

# See service type
ros2 service list -t
```
4. View service type another way
```
# ros2 service type <service_name>

ros2 service type /clear
```
5. Find service with specific type
```
ros2 service find <service_type>

ros2 service find std_srvs/srv/Empty
```
6. See Interface
```
ros2 interface show <service_type>

ros2 interface show turtlesim/srv/Spawn
```
7. Calling a serice
```
ros2 service call <service_name> <service_type>

# Clear drawing
ros2 service call /clear std_srvs/srv/Empty

# Spawn turtle
ros2 service call /spawn turtlesim/srv/Spawn "{x:2,y:2, theta: 0.2, name:''}"
```
-----------------------------------------------------------------------------------
# 5. ROS Parameters
Parameters are values you can change inside a node

1. In a new terminal, run
```
ros2 run turtlesim turtlesim_node
```
2. In another terminal, run
```
ros2 run turtlesim turtle_teleop_key
```
3. See list of parameters
```
ros2 param list
```
4. Get parameter value
```
ros2 param get <node_name> <parameter_name>

ros2 param get /turtlesim background_g
```
5. Set parameter value
```
ros2 param set <node_name> <parameter_name> <value>

ros2 param set /turtlesim background_g 255
```
6. View all param for a node
```
ros2 param dump <node_name>

ros2 param dump /turtlesim
```
7. Store param in yaml file
```
ros2 param dump /turtlesim > turtlesim.yaml
```
8. Load param from yaml file
```
ros2 param load /turtlesim turtlesim.yaml
```
9. Load param on startup
```
ros2 run <package_name> <executable_name> --ros-args --params-file <path_to_params.yaml>

ros2 run turtlesim turtlesim_node --ros-args --params-file turtlesim.yaml
```
----------------------------------------------------------------------------
# 6. ROS Actions
Actions lets you communicate between nodes with a goal, feedback, and result in a client-server fashion.

1. In a new terminal, run
```
ros2 run turtlesim turtlesim_node
```
2. In another terminal, run
```
ros2 run turtlesim turtle_teleop_key
```
3. In the teleop window, try the following and observe
   a. Press on the letter 'G' and see the status when complete
   b. Press one of the letters and cancel early with 'F'
   c. Press one of the letters 'G' and before it finishes press another 'F'
4. See the action clients in node info on the buttom
```
ros2 node info /teleop_turtle
```
5. See all actions
```
ros2 action list

# List with type
ros2 action list -t
```
6. See action info
```
ros2 action info <action_name>

ros2 action info /turtle1/rotate_absolute
```
7. See data type for action
```
ros2 interface show turtlesim/action/rotateAbsolute
```
8. Send action goal
```
ros2 action send_goal <action_name> <action_type> <goal>

ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta:1.57}"
```
-------------------------------------------------------------------------------
# 7. View Logs
Rqt_console is a GUI used to view log messages in ROS

1. Start rqt_console
```
ros2 run rqt_console rqt_console
```
2. Run turtlesim node
```
ros2 run turtlesim turtlesim_node
```
3. run teleop node
```
ros2 run turtlesim turtle_teleop_key
```
4. Crash into wall and see error message. Should see 'info' and 'warining' message.
5. Set log types to only show warning. Will no longer see 'info' message at very start of node.
```
ros2 run turtlesim turtlesim_node --ros-args --log-level WARN
```
--------------------------------------------------------------------------
# Launch Multiple Nodes
Using a launch file, we can startup two nodes with just one command.

1. Launch two nodes
```
ros2 launch <package_name> <launch_arguments>

ros2 launch turtlesim multisim.launch.py
```
2. Details of launch file
```
from launch import LaunchDescription
import launch_ros.actions

def generate_launch_description():
  return LaunchDescription([
    launch_ros.action.Node(
      namespace = "turtlesim1",package= 'turtlesim', executable = 'turtlesim_node', output = 'screen'),
    launch_ros.actions.Node(
      namespace = "turtlesim1",package= 'turtlesim', executable = 'turtlesim_node', output = 'screen')
])
```
--------------------------------------------------------------------------------------
# Rosbag to Record Data And Playback Data

Ros topics can be recorded using ros bags and played back from the recorded bag file.

1. In a new terminal, run
```
ros2 run turtlesim turtlesim_node
```
2. In another terminal, run
```
run turtlesim turtle_teleop_key
```
4. See list of topic names
```
ros2 topic list
```
5. Echo the topic and move the turtle with the teleop node to see it update the command velocity
```
ros2 topic echo <topic_name>

ros2 topic echo /turtle1/cmd_vel
```
6. Record topic. Run ' ctrl + c ' to stop recording. For more topic to record.just add more topic names to the arguments.
```
ros2 bag record -o <folder_name>

ros2 bag record -o bagData /turtle1/cmd_vel
```
7. View bag info
```
ros2 bag info <bag_path>

ros2 bag info bagData
```
8. Playback data
```
ros2 bag play <bag_path>

ros2 bag play bagData
```
-----------------------------------------------------------------------------------
# Build Package with Colcon

A package contains your `` source `` , `` CmakeList.txt `` , `` headers `` , and `` package.xml ``  (meta info) for your ROS programs. Colcon is the tool to build your package.

1. Install colcon
```
sudo apt install python3-colcon-common-extensions
```
2. Create workspace and navigate to workspace
```
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
```
3. Add some source code to test building
```
git clone https://github.com/ros/ros_tutorials.git -b humble
```
4. Setup colcon tab completion
```
echo "source/usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> ~/.bashrc
```
5. Build with colcon from ~ros2_ws. The --symlink-install creates symbolic links (symlinks) to the original files in the source or build directories instead of making copies.
```
colcon build --symlink-install
```
6. See the folders that were created(build, install, log)
```
ls
```
7. Sources overly from ~/ros2_ws
```
source install/local_setup.bash
```
8. Run node
```
ros2 run turtlesim turtlesim_node
```
9. To verify you are modefying your package and not the previous one. let's edit the name of the window. you have to search for 'turtlefram.cpp'. Modify line52 which has windowTitle
```
setWindowTitle("Plawang's TurtleSim")
```
7. Rebuild, source, then run.
  Should see the name change.
```
colcon build --symlink-install
source install/local_setup.bash
ros2 run turtlesim turtlesim_node
```
-----------------------------------------------------------------------------------
# Create Your Own Package

Recall a package contains your `` source `` . `` CmakeList.txt `` , `` headers `` , and `` package.xml `` (meta info) for your ROS programs. Here we will create our own package from scratch.

0. Prereq - install colcon and create workspace. skip if did it already from topic.
```
sudo apt install python3-colcon-common-extensions
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
```
1. Create package from
   ~/ros2 ws/src
```
rosf2 pkg create --build-type amet_cmake --node-name my_node my_package
```
2. Check out the files and folders that it made. Open files and see what's inside.
```
`` include/my_package ``
`` src/my_node.cpp ``
`` cMakeList.txt ``
`` package.xml ``
```
3. Build that package from `` ~/ros_ws ``
```
colcon build --packages-select my_package
```
4. Source
```
source install/local_setup.bash
```
5. Run node
```
ros2 run my_package my_node
```
-----------------------------------------------------------------------------------
# Publisher and Subscriber C++

0. Install CMake extension to view CMakeList.txt files with syntax coloring.
1. Create package from '~/ros2_ws/src'. We will make a cpp_pubsub package folder with the following generated 'include', 'src', 'CMakeList.txt', and 'package.xml'.
```
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_cmake cpp_pubsub
```
2. For the pubsub src, copy 
------------------------------------------------------------------------------------
# Doctor
`` Ros2 doctar `` (aka "wtf - where is the fault") is a diagnostic tool that helps you check if your dependencies or packages are up to date and if there is any communication issues between your nodes.

1. Run ros2 doctor
```
ros2 doctor

#or
ros2 wtf
```
2. In one terminal, run turtlesim_node
```
cd  ~/ros_ws
ros2 run turtlesim turtlesim_node
```
3. In another terminal, run the teleop node
```
cd ~/ros_ws
ros2 run turtlesim turtle_teleop_key
```
4. Run ros2 doctor and see the warnings
```
rosf2 doctor
```
5. Subscribe to one of the topics
```
ros2 topic echo /turtle1/color_sensor
```
6. Run ros2 doctor and should see one of the warning go away
```
ros2 doctor
```
7. See full report
```
ros2 doctor --report
```
--------------------------------------------------------------

# Plugins

Plugins lets you dynamically load new functionality to your code without having to recompile the codes that is using it.

# Create Base Class Package

1. Create base class package
```
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_cmake --dependencies pluginlib --node-name area_node --license Apache-2.0 polygon_base
```
2. Create abstract base class.
   Move `` .../polygon_base/regular_polygon.hpp `` to `` ~/ros2_ws/src/polygon_base/include/polygon_base ``
3. Modify `` CMakeList.txt `` file.

# Create Plugin Package
1. Create `` pllygon_plugin  `` package
```
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_cmake --dependencies polygon_base pluginlib --library-name polygon_plugins --license Apache-2.0 polygon_plugins
```
2. Replace `` ros2_ws/src/polygon_plugins/src/polygon_plugins.cpp `` with `` .../polygon_plugins/polygon_plugins.cpp ``
3. Move .../polygon_plugins/plugins.xml to ros2_ws/src/polygon_plugins
4. Modyfy `` CMakeLists.txt `` file
----------------------------------------------------------------------------------
# Use the plugin
Key Point: the area_node.cpp can use the triangle and square implementation created in the ploygon_plugins without having to include the polygon_plugin class.

1. Replace the
   `` ~/ros2_ws/src/polygon_base/src/area_node.cpp `` with the one in  `` .../polygon_base/area_node.cpp ``
2. Build package
```
cd ~/ros2_ws
colcon build --packages-select polygon_base polygon_plugins
```
3. Run code
```
source install/setup.bash
ros2 run polygon_base area_node
```
--------------------------------------------------------------------------------
# Managing Dependencies with rosdep

# What is rosdep?
The rosdep command is a tool used to manage dependencies in ROS.

# How does rosdep work?
rosdep will find the `` rosdep keys `` (the dependencies listed in the `` package.xml `` file) and check with a central index.

# Types of dependencies in package.xml files
- `` <depend> `` is for dependencies in build time and run time, usually for c++ packages.

- `` <build_depend `` is for dependencies for building your package (not during execution)

- `` <build_export_depend> `` is for external package that depend on this package.

- `` <exec_depend> `` is for run time (shared libraries, executables, python modules, launch scripts)

- `` <test_depend `` is for running tests.

# rosdep installation 
```
apt-get install python3-rosdep
sudo rosdep init
rosdep update
```
# Running the rosdep command
```
cd ~/ros_ws
rosdep install --from-paths src -y --ignore-src
```
Meaning:

- `` --from-path src `` checks package.xml files in the src folder

- `` -y `` install all answer yes to all prompts

- `` --ignore-src `` ignores packages in the src folder

-----------------------------------------------------------------------------------
