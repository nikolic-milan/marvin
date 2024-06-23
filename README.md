# Guide to running the project

This is a simple tutorial walking through how to start a Gazebo simulation of Marvin, controlled via the keyboard.
It assumes that `marvin_gazebo, marvin_control, marvin_description` are in your catkin workspace

It works on Windows via. WSL 2 (Ubuntu) or on PopOS and MacOS.

## Steps

1. [Install **docker**](<Install docker.md>)
1. **Build** the docker image by running
    ```
    ./build.sh
    ```
1. To create and run the `my_ros` container, run the script:
    ```bash
    ./run.sh
    ```
    **Note:** This will stop and recreate the container, if it already exists

1. **Initialize and build the project** by running the following commands:
    ```bash
    cd ~/catkin_ws/
    sudo catkin build
    source ./devel/setup.bash
    ```

1. Run **gazebo, rviz and topic publisher** using
    ```
    roslaunch marvin_control marvin_skid_teleop.launch
    ```
    or
    ```
    roslaunch carbot_gazebo_control carbot_gazebo_control.launch
    ```


    This will start...
    - **rviz** which will show the positions of the robot in real time
    - **gazebo** which will be running the simulation
    - **publishing** to topic `marvin/cmd_vel`

### Skid Steer
1. Now you need a **second terminal** instance for running **teleop control**, either via a TMUX session or by opening a new terminal and running
    ```bash
    ./connect.sh
    ```

1. To **initialize the second terminal**, you don't need to rebuild, just run:
    ```bash
    cd ~/catkin_ws/
    source ./devel/setup.bash
    ```

1. To **start the teleop control**, run:

    ```bash
    rosrun teleop_twist_keyboard teleop_twist_keyboard.py cmd_vel:=/marvin/cmd_vel
    ```
    You will know this is successfull if you see small table with explanations for control of the vehicle.

or

1. To **start the movement planner**
    ```bash
    rosrun marvin_plan movement_to_point.py
    ```
### Ackermann Steer

1. Open a new terminal, and navigate to the source folder, and run.

    ```bash
    catkin build
    source devel/setup.bash
    ```
2. Start publishing commands for movement.

    ```bash
    # Stop the robot.
    rostopic pub /cmd_vel geometry_msgs/Twist "{linear: {x: 0.0, y: 0.0}, angular: {z: 0.0}}" -1
    ```

    ```bash
    # Move straight
    rostopic pub /cmd_vel geometry_msgs/Twist "{linear: {x: 0.1}}"
    ```

    ```bash
    # Move straight
    rostopic pub /cmd_vel geometry_msgs/Twist "{linear: {x: 0.1}}"
    ```

    ```bash
    # Move straight and turn left
    rostopic pub /cmd_vel geometry_msgs/Twist "{linear: {x: 0.1}, angular: {z: 0.02}}"
    ```

    ```bash
    # Move straight and turn right
    rostopic pub /cmd_vel geometry_msgs/Twist "{linear: {x: 0.1}, angular: {z: -0.02}}"
    ```
