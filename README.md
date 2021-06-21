# ROS_DownloadAndMore
This repository is for task 1 of the AI track of my summer training at Smart-Methods

NOTE: I used windows 10 - Make sure your device hits the minimum requiremnt, you can check through this link:
"https://askubuntu.com/questions/1030839/minimum-requirements"

----------------------------------------------------------------------------------

To be able to run ROS youre required first to download VB via:
"https://www.virtualbox.org/wiki/Downloads"

----------------------------------------------------------------------------------

Then craete an Ubuntu 18.04 system in it and run it, the following link is for dounloading Ubuntu 18.04:
"https://releases.ubuntu.com/18.04/" 

After creating a user and setting a password of Ubuntu, login to your system.

----------------------------------------------------------------------------------

Download ROS Melodic OR Neotic ( Kinetic isnt reccomended ) " http://wiki.ros.org/ROS/Installation ", 
choose Ubuntu platform NOT windows and then follow the given installation instructions,
To be specific, theese are the following codes I used in the same order:

$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

$ curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
	NOTE: You might encounter an error says "curl command not found" no worries it just means that 'curl' package is not installed on your Ubuntu machine.
you can solve it by the command "$ sudo apt install curl "

$ sudo apt update

$ sudo apt install ros-melodic-desktop-full

$ echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

$ source ~/.bashrc

$ sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential

$ sudo apt install python-rosdep

$ sudo rosdep init

$ rosdep update

Make sure everything is done in ROS setup environment using the following code:

$ printenv | grep ROS

---------------------------------------------------------------------------------------------

Now we are required to create what is called a Workspace directory type in Ubuntu system's terminal:

1- Create a WorkSpace: 
$ pwd 
$ source /opt/ros/melodic/setup.bash
$ mkdir -p ~/catkin_ws/src

2- Check if it was crerated: 
$ ls

3- Then:
$ cd ~/catkin_ws/
$ catkin_make
$ source devel/setup.bash

Now, check where the WorkSpace is installed (path)- type: (melodic)
$ echo $ROS_PACKAGE_PATH

To run ROS we use what is called a master node through terminal command:
$ roscore

Open new terminal then: 
$ rosenode list
$ rosetopic list

	NOTE: Anytime you need to check which version of ROS you've installed, use the following code in the terminal:
	$ rosversion -d

------------------------------------------------------------------------------------

Next, we will be working on Installing the package: "arduino_robot_arm" which belongs to the Smart-Methods co.

- Add the “arduino_robot_arm” package to “src” folder
$ cd ~/catkin_ws/src
$ sudo apt install git
$ git clone https://github.com/smart-methods/arduino_robot_arm 

- Install all the dependencies 
$ cd ~/catkin_ws
$ rosdep install --from-paths src --ignore-src -r -y
$ sudo apt-get install ros-melodic-moveit
$ sudo apt-get install ros-melodic-joint-state-publisher ros-melodic-joint-state-publisher-gui
$ sudo apt-get install ros-melodic-gazebo-ros-control joint-state-publisher
$ sudo apt-get install ros-melodic-ros-controllers ros-melodic-ros-control
	NOTE: Replace the word "melodic" with your version if you dont use melodic.

-Now an error might appear: [check_motors.launch] is neither a launch file in package.. It can be solved using the following code: 
$ sudo nano ~/.bashrc


<< NOW SCROLL UNTILL THE END OF LINE >>
at the end of the (bashrc) file add the follwing line

CHANGE "weiam" TO YOUR SYSTEM NAME
(source /home/weiam/catkin_ws/devel/setup.bash)

then 
  ctrl + o
then 
  Enter
then 
  crtl + x

$ source ~/.bashrc

Then, the following code runs Rviz:
$ roslaunch robot_arm_pkg check_motors.launch

----------------------------------------------------------------------------------

Download Arduino IDE on your Ubuntu system (64-bits) using this link for their official website: " https://www.arduino.cc/en/Main/Software" 

since installing the the arduino libraries did not worked using the "BINARY" I used the "SOURCE" method: 
$ cd ~/catkin_ws 
$ cd src 
$ git clone https://github.com/ros-drivers/rosserial.git
$ cd ..
$ pwd
$ catkin_make 
$ catkin_make install

In a new terminal enter the following codes in the same order:
$ pwd 
$ cd .. 
$ cd ..
$ pwd
$ ls
$ cd home 
$ cd weiam << CHANGE "weiam" TO YOUR SYSTEM NAME
$ ls
$ cd Arduino 
$ ls 
$ cd libraries 
$ pwd 
$ clear
$ pwd 
$ rm -rf ros_lib
$ rosrun rosserial_arduino make_libraries.py .

Restart Arduino IDE sortware; Then go to "Files" tab then "Examples" then "ros_lib" is appeared.

-------------------------------------------------------------------------------------------------------------------------------------

To control the motors in simulation, run the following codes in the terminal: 

$ roslaunch robot_arm_pkg check_motors.launch
$ roslaunch robot_arm_pkg check_motors_gazebo.launch
$ rosrun robot_arm_pkg joint_states_to_gazebo.py

You may need to change the permission 
	$ cd catkin/src/arduino_robot_arm/robot_arm_pkg/scripts
	$ sudo chmod +x joint_states_to_gazebo.py

--------------------------------------------------------------------------------------------------------------

These codes will start and run moveIt:

$ roslaunch moveit_setup_assistant setup_assistant.launch
$ roslaunch moveit_pkg demo.launch

Then to open Gazebo with our packages, run the following code:
$ roslaunch moveit_pkg demo_gazebo.launch
