# GA-DRL algorithm with robotic manipulator Aubo i5

## Prerequisite
- Must have compiled the aubo robot github repo under the kinetic branch,which can be found here:
  - It is safe to remove auto_controller folder if you get build error with this package
```
https://github.com/adarshsehgal/aubo_robot
```
- Ubuntu 16.04
- Ros Kinetic
- Python 2.7
- Python verison >= 3.5 (this code was run on python 3.5)
- Aubo gym environment uses python2.7 with moveit
- Genetic Algorithm ga.py uses python3.7
- pip install gym==0.15.6
- Install the packages needed to install gym
```
pip3 install scipy tqdm joblib cloudpickle click opencv-python
```
- pip install tensorflow==1.14.0
- openai_ros
  - IMPORTANT: run rosdep install openai_ros EACH time you run the code (for each terminal)
```
https://github.com/adarshsehgal/openai_ros
```
- update pip3 to 21.0 or latest (if no errors)
```
pip3 install --upgrade pip
```
- To avoid libmoveit_robot_trajectory.so error, follow below commands
  - replace the version number of 0.9.17 with what you have in below directory
  - don’t change 0.9.15 
```
cd /opt/ros/kinetic/lib 
sudo cp -r libmoveit_robot_trajectory.so.0.9.17 .. 
sudo cp -r libmoveit_robot_state.so.0.9.17 ..
cd .. 
sudo mv libmoveit_robot_state.so.0.9.17 libmoveit_robot_state.so.0.9.15 
sudo mv libmoveit_robot_model.so.0.9.17 libmoveit_robot_model.so.0.9.15
sudo mv libmoveit_robot_trajectory.so.0.9.17 libmoveit_robot_trajectory.so.0.9.15
sudo cp -r libmoveit_robot_state.so.0.9.15 lib/ 
sudo cp -r libmoveit_robot_model.so.0.9.15 lib/ 
sudo cp -r libmoveit_robot_trajectory.so.0.9.15 lib/
```
- To avoid error with installation of mpi4py package
```
sudo apt install libpython3.7-dev
pip3 install mpi4py
```
- No need to install mujoco-py, since the code uses Rviz
- Genetic algorithm library
```
pip3 install https://github.com/chovanecm/python-genetic-algorithm/archive/master.zip#egg=mchgenalg
https://github.com/adarshsehgal/python-genetic-algorithm.git
```


## How to run the program


**Before running roslaunch command, setup aubo robot repository with ros kinetic (link in pre requite section)**
```
cd catkin_workspace
catkin build
source devel/setup.bash
rosdep install openai_ros
```

**Launch rviz and moveit by either launch commands:** 

**For simulation:**
```
roslaunch aubo_i5_moveit_config demo.launch
```
For real robot:
```
roslaunch aubo_i5_robot_config moveit_planning_execution.launch robot_ip:=<your robot ip>
```
Go into the newher directory - `cd newher`

To run the connection between the robot gym environment and moveit run:
```
python2.7 moveit_motion_control.py
```

Run the genetic algorithm on her+ddpg while still in newher directory:
```
python3 ga.py
```

To make the program run faster, you can comment rviz below code in aubo robot repository:
```
cd ~/catkin_workspace/src/aubo_robot/aubo_i5_moveit_config/launch
gedit demo.launch (or just manually open demo.launch in any text editory)

 <!--<include file="$(find aubo_i5_moveit_config)/launch/moveit_rviz.launch">
    <arg name="config" value="true"/>
    <arg name="debug" value="$(arg debug)"/>
  </include> -->
```

Gym Environments available for GA-DRL execution (can be changes in ga.py):
- AuboReach-v0 - executes joint states with moveit
- AuboReach-v1 - only calculates actions but does not execute joint states (increased learning speed)