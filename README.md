# Ego Planner Swarm ✈️

This repository is based on the [Ego-Planner-Swarm](https://github.com/ZJU-FAST-Lab/ego-planner-swarm.git) project. We would like to express our gratitude to the ZJU FAST Lab for their excellent work and for making their code available to the community.

# Quick Start

### Step 1: Add ego_planner_swarm to your workspace

Download this repo and add to your own workspace：

```bash
your_ws/src/ego_planner_swarm
```

### Step 2: Creat a launch file

Here is a example to launch ego-planner-swarm. PCD map and Drone init position can be customized. If you want to customize other attributes like maximum speed, you can find them in ```ego_planner_swarm/planner/plan_manage/launch/run_in_sim.launch```.

```xml
<launch>
    <!-- Map Config -->
    <arg name="map_size_x" value="60.0"/>
    <arg name="map_size_y" value="40.0"/>
    <arg name="map_size_z" value="10.0"/>
    <node pkg="map_generator" name="map_pub" type="map_pub" output="screen" args="$(find your_pkg)/files/map.pcd" />

    <!-- Robot Config -->
    <arg name="odom_topic" value="visual_slam/odom" />

    <include file="$(find ego_planner)/launch/run_in_sim.launch">
        <arg name="drone_id" value="0"/>

        <arg name="init_x" value="-10.0"/>
        <arg name="init_y" value="-15.0"/>
        <arg name="init_z" value="1"/>

        <arg name="map_size_x" value="$(arg map_size_x)"/>
        <arg name="map_size_y" value="$(arg map_size_y)"/>
        <arg name="map_size_z" value="$(arg map_size_z)"/>
        <arg name="odom_topic" value="$(arg odom_topic)"/>
    </include>

    <include file="$(find ego_planner)/launch/run_in_sim.launch">
        <arg name="drone_id" value="1"/>

        <arg name="init_x" value="-10.0"/>
        <arg name="init_y" value="-16.0"/>
        <arg name="init_z" value="1"/>

        <arg name="map_size_x" value="$(arg map_size_x)"/>
        <arg name="map_size_y" value="$(arg map_size_y)"/>
        <arg name="map_size_z" value="$(arg map_size_z)"/>
        <arg name="odom_topic" value="$(arg odom_topic)"/>
    </include>

    ...

</launch>
```

### Step 3: Publish waypoint and subscribe odom

Here is an example to publish waypoint and subscribe odom. Be careful about the topic name, where you should replace ```{id}``` with the target drone id you want to control. ```{id}``` and ```{odom_topic}``` is defined in your launch file.

```python
self.waypoint_pub = rospy.Publisher(f"/drone_{id}_planning/waypoint", PoseStamped, queue_size=10)
self.odom_sub = rospy.Subscriber(f"/drone_{id}_{odom_topic}", Odometry, self._odom_cb)
```
