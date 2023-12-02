# nav2-demo

ROS 2 Navigation bemutatása a Robotirányítás rendszertechnikája gyakorlat keretében.

1. Turtlebot3 szimuláció elindítása, robot távirányítása billentyűzetről
   ```
   ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
   ros2 run turtlebot3_teleop teleop_keyboard
   ```
   - Milyen node-okból áll a rendszer?
   - Milyen topic-ok vannak?
     - **cmd_vel** megmutatása parancssorban, miközben távirányítjuk egy másik terminálból a robotot
     - **odom** topic megmutatása parancssorban
     - **lidar mérések** megmutatása parancssorban
   - TF fa kirajzolása:
   ```
   ros2 run tf2_tools view_frames
   evince frames.pdf
   ```
   - RViz-ben (config: robot_teleop.rviz):
     - Robotmodell, lidar mérések
     - TF bekapcsolása:
       - base_link
       - base_scan
       - odom
       - base_footrpint
     - Fixed frame: odom

3. Navigáció bemutatása
   ```
   ros2 launch nav2_demo tb3_nav_sim_launch.py
   ```
   - Milyen node-okat látunk?
     - gazebo (ez szimulálja a robotot)
     - turtlebot3\_diff\_drive
     - turtlebot3_laserscan
     - amcl (lokalizáció)
     - map_server
     - global_costmap
     - planner_server
     - local_costmap
     - controller_server
     - bt_navigator
   - Milyen action-öket látunk?
   ```
   ros2 action list -t
   ros2 action info /navigate_to_pose
   ros2 interface show nav2_msgs/action/NavigateToPose
   ```
   - RViz-ben:
     - Robot, lidar
     - Előre rögzített térkép
     - Lokalizáció lidar mérések és térkép alapján - AMCL
     - TF-ek megmutatása (map, odom, base link, stb.)
     - Planner (globális mozgástervezés)
       - Global costmap
       - Dijkstra algoritmussal megtervezett pálya
     - Controller (lokális akadályelkerülés)
       - Local costmap
       - DWA algoritmus generálja a sebességparancsokat


## Felhasznált források

[Open Navigation LLC: NAV2 - Getting started](https://navigation.ros.org/getting_started/index.html)

[The Robotics Back-End: ROS2 Nav2 Tutorial](https://roboticsbackend.com/ros2-nav2-tutorial/)


## Hasznos parancsok

Moving around:

```
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
ros2 run turtlebot3_teleop teleop_keyboard
```

SLAM:

```
ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True
ros2 run nav2_map_server map_saver_cli -f tmp/my_map
```

Navigate:

```
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True
```