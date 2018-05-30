# Turtlebot 3 Autorace simulation #

Turtlebot 3 Autorace simulation is a ROS package which allows to run ```turtlebot3_autorace``` from ROBOTIS-GIT in simulation. It is fully parametrizable and customizable. Next will follow the instructions to getting started with standard examples and to customize trucks to test the package.

## Getting Started ##

It is supposed you have Ubuntu 16.04 with ROS Kinetic installed.

First of all you need to install dependent packages for TurtleBot3 control.
Do the followings command in order to install it.

```shell
sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server ros-kinetic-move-base ros-kinetic-urdf ros-kinetic-xacro ros-kinetic-compressed-image-transport ros-kinetic-rqt-image-view ros-kinetic-gmapping ros-kinetic-navigation ros-kinetic-interactive-markers
```

Next you need the packages with the turtlebot robot model, turlebot simulation environment and the turtlebot autorace software. In order to get these do the followings commands:

- Move in your ```src``` folder on your ROS workspace. In these README will be used ```catkin_ws``` as the workspace name.

```bash
cd ~/catkin_ws/src/
```

- Clone all the repositories listed below and build everything

```bash
git clone https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
git clone https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
git clone https://github.com/ROBOTIS-GIT/turtlebot3_autorace.git
<!-- TODO: add clone to this repository -->
cd ~/catkin_ws && catkin_make
```

This repository contains a folder named ```world``` which contains the gazebo simulation environments. In order to makes it work you need to copy the environments inside the gazebo models directory. In order to do that do the following commands:

```bash
roscd turtlebot3_autorace_simulation
cp -r ./world/turtlebot3_autorace_track* $HOME/.gazebo/models
```

Now these models will be avaible inside shapes models.

This repository use a custom model of turtlebot3. This model is the turtlebot3 burger pi. In order to have it inside the simulation environment it has to be added to the models description in the ```turtlebot3_descripion``` package.
The 3D model and robot description is inside the ```urdf``` folder. In order to add it to the choosable models do the following steps:

```bash
cp ./urdf/turtlebot3_burger_pi* $HOME/catkin_ws/src/turtlebot3/turtlebot3_description/urdf/.
```

Now to tell which model you want to use you need to set the variable with this command:

```bash
export TURTLEBOT3_MODEL=burger_pi
```

It is suggested to add the command below to your `.bashrc` or you will need to export the variable everytime a new terminal is opened.

## Start Autorace ##

Let's get autodriving!

### Simulation environment ###

In order to launch the simulation environment, a ROS launch file is provided. Do the following command:

```bash
roslaunch turtlebot3_autorace_simulation gazebo.launch
```

This launch file can takes the following arguments:

1. ```x_pos```: set the x position of the robot.
2. ```x_pos```: set the x position of the robot.
3. ```x_pos```: set the x position of the robot.
4. ```track```: choose the track name. In this repository track1 and track2 are provided.

### Controller ###

The robot need a controller to move. It is provided with a ROS launch file. Do the following command:

```bash
roslaunch turtlebot3_autorace_simulation fake_controller.launch
```

### Autorace nodes ###

The robot need also the autorace nodes which detect the lane and gives the needed velocity command.
The autorace nodes are composed by three subnodes which do the image projection and then detect the lanes.
Also in this case a ROS launch file is provided:

```bash
roslaunch turtlebot3_autorace_simulation autorace.launch
```

Now the robot will move and will follow the lanes.

<!-- ## Calibration ##

Do the robot do not follow the lanes? You need to calibrate it.
The roslaunch files will search inside the ```config``` folder in this repository. Then you need to override the files inside that folder.
In order to enter configuration mode you can pass as parameter to the autorace launch the ```calibration_mode``` argument. In order to do this do the following step:

```bash
roslaunch turtlebot3_autorace_simulation gazebo.launch
roslaunch turtlebot3_autorace_simulation autorace.launch calibration_mode:=calibration
```

Then open ```rqt_image_view``` to see what the camera sees. If the calibration mode is on you shoud see wrong detect in the topic -->
