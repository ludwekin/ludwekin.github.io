---
title: 用gazebo进行气球/uav追踪仿真
published: 2026-01-19T07:47:49.368Z
category: 四旋翼
tags:
  - 仿真
  - gazebo
  - 气球
  - ubuntu22
  - ros2
draft: true
---

## 目的

尝试一下gazebo能不能合理仿真，然后部署一个随机飘动的气球，并用一台uav来追踪它。测试追踪算法。

如果气球仿真成功，那就换成一个uav在天上随机飞行，且用另外一台uav来跟踪它。测试追踪算法。

## 开发/部署,系统环境 

开发环境为 Ubuntu 22.04（非虚拟机）、ROS 2 Humble 和 Ignition Gazebo Fortress。


## 搭建python/ros2环境


确认系统环境。ign gazebo --versions.

6.17.0.



安装Python依赖。

sudo apt install python3-rosdep2.

创建工作空间。

mkdir -p ~/balloon_ws/src.

cd ~/balloon_ws/src.

创建ROS 2包。

ros2 pkg create random_balloon_gazebo --build-type ament_python --dependencies rclpy gazebo_ros std_msgs geometry_msgs.

cd ~/balloon_ws.

colcon build.

source install/setup.bash.




ign gazebo -v 4 empty.sdf.

可以运行。

vim box_world.sdf.


<?xml version="1.0" ?>
<sdf version="1.7">
  <world name="demo_world">

    <!-- 物理引擎 -->
    <physics type="ode">
      <max_step_size>0.001</max_step_size>
      <real_time_factor>1.0</real_time_factor>
    </physics>

    <!-- 地面 -->
    <include>
      <uri>https://fuel.gazebosim.org/1.0/openrobotics/models/Ground Plane</uri>
    </include>

    <!-- 光源 -->
    <include>
      <uri>https://fuel.gazebosim.org/1.0/openrobotics/models/Sun</uri>
    </include>

    <!-- 一个盒子 -->
    <model name="box">
      <pose>0 0 0.5 0 0 0</pose>
      <link name="link">
        <collision name="collision">
          <geometry>
            <box><size>1 1 1</size></box>
          </geometry>
        </collision>
        <visual name="visual">
          <geometry>
            <box><size>1 1 1</size></box>
          </geometry>
        </visual>
      </link>
    </model>

  </world>
</sdf>


可以运行，成功得到黑盒子。






## ai prompt


1. 开发环境为 Ubuntu 22.04（非虚拟机）、ROS 2 Humble 和 Ignition Gazebo Fortress。

2. 对于gazebo仿真，ai需要看的文档是gazebo 6.17.0。

3. ApplyLinkWrench is a world-level plugin — it must be placed directly under <world>, not inside a <model>.








## 一些疑问

1. 2026年，gazebo有哪些替代？

    Gazebo Sim是ROS 2生态中最主流的开源机器人仿真器。

    NVIDIA Isaac Sim也很不错。而且还适合强化学习。

2. gazebo到底是什么？

    Gazebo 是完全开源的。由 Open Robotics 主导开发。源代码也完全公开在 GitHub 上。

3. gazebo和ros2的关系？

    Gazebo 是 ROS 生态的默认/官方推荐模拟器。

4. 如何确定 当前系统/开发环境 适配的gazebo版本？

    Ubuntu 22.04 LTS + ROS 2 Humble (LTS) + Ignition Gazebo Fortress.

    Ubuntu 24.04 + ROS 2 Jazzy + Ignition Gazebo Harmonic.



## 仿真前测试/demo1 仿真一个小车拉着气球跑

<?xml version="1.0" ?>
<sdf version="1.8">
    <world name="car_world">
        <physics name="1ms" type="ignored">
            <max_step_size>0.001</max_step_size>
            <real_time_factor>1.0</real_time_factor>
        </physics>
        <plugin
            filename="libignition-gazebo-physics-system.so"
            name="ignition::gazebo::systems::Physics">
        </plugin>
        <plugin
            filename="libignition-gazebo-user-commands-system.so"
            name="ignition::gazebo::systems::UserCommands">
        </plugin>
        <plugin
            filename="libignition-gazebo-scene-broadcaster-system.so"
            name="ignition::gazebo::systems::SceneBroadcaster">
        </plugin>

        <!-- 空气浮力系统 -->
        <plugin
            filename="libignition-gazebo-buoyancy-system.so"
            name="ignition::gazebo::systems::Buoyancy">
            <uniform_fluid_density>1.225</uniform_fluid_density>
        </plugin>

        <light type="directional" name="sun">
            <cast_shadows>true</cast_shadows>
            <pose>0 0 10 0 0 0</pose>
            <diffuse>0.8 0.8 0.8 1</diffuse>
            <specular>0.2 0.2 0.2 1</specular>
            <attenuation>
                <range>1000</range>
                <constant>0.9</constant>
                <linear>0.01</linear>
                <quadratic>0.001</quadratic>
            </attenuation>
            <direction>-0.5 0.1 -0.9</direction>
        </light>

        <model name="ground_plane">
            <static>true</static>
            <link name="link">
                <collision name="collision">
                <geometry>
                    <plane>
                    <normal>0 0 1</normal>
                    </plane>
                </geometry>
                </collision>
                <visual name="visual">
                <geometry>
                    <plane>
                    <normal>0 0 1</normal>
                    <size>100 100</size>
                    </plane>
                </geometry>
                <material>
                    <ambient>0.8 0.8 0.8 1</ambient>
                    <diffuse>0.8 0.8 0.8 1</diffuse>
                    <specular>0.8 0.8 0.8 1</specular>
                </material>
                </visual>
            </link>
        </model>

        <model name='vehicle_blue' canonical_link='chassis'>
            <pose relative_to='world'>0 0 0 0 0 0</pose>
            
            <link name='chassis'>
                <pose relative_to='__model__'>0.5 0 0.4 0 0 0</pose>
                <inertial>
                    <mass>1.14395</mass>
                    <inertia>
                        <ixx>0.095329</ixx>
                        <ixy>0</ixy>
                        <ixz>0</ixz>
                        <iyy>0.381317</iyy>
                        <iyz>0</iyz>
                        <izz>0.476646</izz>
                    </inertia>
                </inertial>
                <visual name='visual'>
                    <geometry>
                        <box>
                            <size>2.0 1.0 0.5</size>
                        </box>
                    </geometry>
                    <material>
                        <ambient>0.0 0.0 1.0 1</ambient>
                        <diffuse>0.0 0.0 1.0 1</diffuse>
                        <specular>0.0 0.0 1.0 1</specular>
                    </material>
                </visual>
                <collision name='collision'>
                    <geometry>
                        <box>
                            <size>2.0 1.0 0.5</size>
                        </box>
                    </geometry>
                </collision>
            </link>

            <link name='left_wheel'>
                <pose relative_to="chassis">-0.5 0.6 0 -1.5707 0 0</pose>
                <inertial>
                    <mass>1</mass>
                    <inertia>
                        <ixx>0.043333</ixx>
                        <ixy>0</ixy>
                        <ixz>0</ixz>
                        <iyy>0.043333</iyy>
                        <iyz>0</iyz>
                        <izz>0.08</izz>
                    </inertia>
                </inertial>
                <visual name='visual'>
                    <geometry>
                        <cylinder>
                            <radius>0.4</radius>
                            <length>0.2</length>
                        </cylinder>
                    </geometry>
                    <material>
                        <ambient>1.0 0.0 0.0 0</ambient>
                        <diffuse>1.0 0.0 0.0 0</diffuse>
                        <specular>1.0 0.0 0.0 0</specular>
                    </material>
                </visual>
                <collision name='collision'>
                    <geometry>
                        <cylinder>
                            <radius>0.4</radius>
                            <length>0.2</length>
                        </cylinder>
                    </geometry>
                </collision>
            </link>

            <link name='right_wheel'>
                <pose relative_to="chassis">-0.5 -0.6 0 -1.5707 0 0</pose>
                <inertial>
                    <mass>1</mass>
                    <inertia>
                        <ixx>0.043333</ixx>
                        <ixy>0</ixy>
                        <ixz>0</ixz>
                        <iyy>0.043333</iyy>
                        <iyz>0</iyz>
                        <izz>0.08</izz>
                    </inertia>
                </inertial>
                <visual name='visual'>
                    <geometry>
                        <cylinder>
                            <radius>0.4</radius>
                            <length>0.2</length>
                        </cylinder>
                    </geometry>
                    <material>
                        <ambient>1.0 0.0 0.0 0</ambient>
                        <diffuse>1.0 0.0 0.0 0</diffuse>
                        <specular>1.0 0.0 0.0 0</specular>
                    </material>
                </visual>
                <collision name='collision'>
                    <geometry>
                        <cylinder>
                            <radius>0.4</radius>
                            <length>0.2</length>
                        </cylinder>
                    </geometry>
                </collision>
            </link>

            <frame name="caster_frame" attached_to='chassis'>
                <pose>0.8 0 -0.2 0 0 0</pose>
            </frame>

            <!-- 气球绳子 -->
            <link name='balloon_string'>
                <pose relative_to="chassis">0 0 4.25 0 0 0</pose>
                <inertial>
                    <mass>0.005</mass>
                    <inertia>
                        <ixx>0.001</ixx>
                        <ixy>0</ixy>
                        <ixz>0</ixz>
                        <iyy>0.001</iyy>
                        <iyz>0</iyz>
                        <izz>0.001</izz>
                    </inertia>
                </inertial>
                <visual name='visual'>
                    <geometry>
                        <cylinder>
                            <radius>0.005</radius>
                            <length>8.0</length>
                        </cylinder>
                    </geometry>
                    <material>
                        <ambient>0.8 0.8 0.8 1</ambient>
                        <diffuse>0.8 0.8 0.8 1</diffuse>
                        <specular>0.8 0.8 0.8 1</specular>
                    </material>
                </visual>
                <collision name='collision'>
                    <geometry>
                        <cylinder>
                            <radius>0.005</radius>
                            <length>8.0</length>
                        </cylinder>
                    </geometry>
                </collision>
            </link>

            <!-- 气球 -->
            <link name='balloon'>
                <pose relative_to="balloon_string">0 0 4.3 0 0 0</pose>
                <inertial>
                    <mass>0.05</mass>
                    <inertia>
                        <ixx>0.001</ixx>
                        <ixy>0</ixy>
                        <ixz>0</ixz>
                        <iyy>0.001</iyy>
                        <iyz>0</iyz>
                        <izz>0.001</izz>
                    </inertia>
                </inertial>
                <visual name='visual'>
                    <geometry>
                        <sphere>
                            <radius>0.3</radius>
                        </sphere>
                    </geometry>
                    <material>
                        <ambient>1.0 0.0 1.0 1</ambient>
                        <diffuse>1.0 0.0 1.0 1</diffuse>
                        <specular>1.0 0.0 1.0 1</specular>
                    </material>
                </visual>
                <collision name='collision'>
                    <geometry>
                        <sphere>
                            <radius>0.3</radius>
                        </sphere>
                    </geometry>
                </collision>
            </link>

            <link name='caster'>
                <pose relative_to='caster_frame'/>
                <inertial>
                    <mass>1</mass>
                    <inertia>
                        <ixx>0.016</ixx>
                        <ixy>0</ixy>
                        <ixz>0</ixz>
                        <iyy>0.016</iyy>
                        <iyz>0</iyz>
                        <izz>0.016</izz>
                    </inertia>
                </inertial>
                <visual name='visual'>
                    <geometry>
                        <sphere>
                            <radius>0.2</radius>
                        </sphere>
                    </geometry>
                    <material>
                        <ambient>0.0 1 0.0 0</ambient>
                        <diffuse>0.0 1 0.0 0</diffuse>
                        <specular>0.0 1 0.0 0</specular>
                    </material>
                </visual>
                <collision name='collision'>
                    <geometry>
                        <sphere>
                            <radius>0.2</radius>
                        </sphere>
                    </geometry>
                </collision>
            </link>

            <joint name='left_wheel_joint' type='revolute'>
                <pose relative_to='left_wheel'/>
                <parent>chassis</parent>
                <child>left_wheel</child>
                <axis>
                    <xyz expressed_in='__model__'>0 1 0</xyz>
                    <limit>
                        <lower>-1.79769e+308</lower>
                        <upper>1.79769e+308</upper>
                    </limit>
                </axis>
            </joint>

            <joint name='right_wheel_joint' type='revolute'>
                <pose relative_to='right_wheel'/>
                <parent>chassis</parent>
                <child>right_wheel</child>
                <axis>
                    <xyz expressed_in='__model__'>0 1 0</xyz>
                    <limit>
                        <lower>-1.79769e+308</lower>
                        <upper>1.79769e+308</upper>
                    </limit>
                </axis>
            </joint>

            <joint name='caster_wheel' type='ball'>
                <parent>chassis</parent>
                <child>caster</child>
            </joint>

            <!-- 绳子连接到底盘 -->
            <joint name='string_to_chassis' type='ball'>
                <parent>chassis</parent>
                <child>balloon_string</child>
                <pose relative_to='balloon_string'>0 0 -4.0 0 0 0</pose>
            </joint>

            <!-- 气球连接到绳子 -->
            <joint name='balloon_to_string' type='ball'>
                <parent>balloon_string</parent>
                <child>balloon</child>
                <pose relative_to='balloon'>0 0 -0.3 0 0 0</pose>
            </joint>

            <plugin
                filename="libignition-gazebo-diff-drive-system.so"
                name="ignition::gazebo::systems::DiffDrive">
                <left_joint>left_wheel_joint</left_joint>
                <right_joint>right_wheel_joint</right_joint>
                <wheel_separation>1.2</wheel_separation>
                <wheel_radius>0.4</wheel_radius>
                <odom_publish_frequency>1</odom_publish_frequency>
                <topic>cmd_vel</topic>
            </plugin>
        </model>

        <!-- Moving Forward -->
        <plugin filename="libignition-gazebo-triggered-publisher-system.so"
                name="ignition::gazebo::systems::TriggeredPublisher">
            <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
                <match field="data">16777235</match>
            </input>
            <output type="ignition.msgs.Twist" topic="/cmd_vel">
                linear: {x: 1.5}, angular: {z: 0.0}
            </output>
        </plugin>

        <!-- Moving Backward -->
        <plugin filename="libignition-gazebo-triggered-publisher-system.so"
                name="ignition::gazebo::systems::TriggeredPublisher">
            <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
                <match field="data">16777237</match>
            </input>
            <output type="ignition.msgs.Twist" topic="/cmd_vel">
                linear: {x: -1.5}, angular: {z: 0.0}
            </output>
        </plugin>

        <!-- Turn Left -->
        <plugin filename="libignition-gazebo-triggered-publisher-system.so"
                name="ignition::gazebo::systems::TriggeredPublisher">
            <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
                <match field="data">16777234</match>
            </input>
            <output type="ignition.msgs.Twist" topic="/cmd_vel">
                linear: {x: 0.0}, angular: {z: 1.5}
            </output>
        </plugin>

        <!-- Turn Right -->
        <plugin filename="libignition-gazebo-triggered-publisher-system.so"
                name="ignition::gazebo::systems::TriggeredPublisher">
            <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
                <match field="data">16777236</match>
            </input>
            <output type="ignition.msgs.Twist" topic="/cmd_vel">
                linear: {x: 0.0}, angular: {z: -1.5}
            </output>
        </plugin>
    </world>
</sdf>



## 正式仿真


<?xml version="1.0" ?>
<sdf version="1.8">
  <world name="sky_world">

    <physics name="1ms" type="ignored">
      <max_step_size>0.001</max_step_size>
      <real_time_factor>1.0</real_time_factor>
    </physics>

    <plugin filename="libignition-gazebo-physics-system.so"
            name="ignition::gazebo::systems::Physics"/>
    <plugin filename="libignition-gazebo-user-commands-system.so"
            name="ignition::gazebo::systems::UserCommands"/>
    <plugin filename="libignition-gazebo-scene-broadcaster-system.so"
            name="ignition::gazebo::systems::SceneBroadcaster"/>

    <plugin filename="libignition-gazebo-buoyancy-system.so"
            name="ignition::gazebo::systems::Buoyancy">
      <uniform_fluid_density>1.225</uniform_fluid_density>
    </plugin>

    <plugin filename="libignition-gazebo-apply-link-wrench-system.so"
            name="ignition::gazebo::systems::ApplyLinkWrench"/>

    <light type="directional" name="sun">
      <cast_shadows>true</cast_shadows>
      <pose>0 0 10 0 0 0</pose>
      <diffuse>0.8 0.8 0.8 1</diffuse>
      <specular>0.2 0.2 0.2 1</specular>
      <attenuation>
        <range>1000</range>
        <constant>0.9</constant>
        <linear>0.01</linear>
        <quadratic>0.001</quadratic>
      </attenuation>
      <direction>-0.5 0.1 -0.9</direction>
    </light>

    <model name="ground_plane">
      <static>true</static>
      <link name="link">
        <collision name="collision">
          <geometry>
            <plane><normal>0 0 1</normal></plane>
          </geometry>
        </collision>
        <visual name="visual">
          <geometry>
            <plane><normal>0 0 1</normal><size>100 100</size></plane>
          </geometry>
          <material>
            <ambient>0.2 0.8 0.2 1</ambient>
            <diffuse>0.2 0.8 0.2 1</diffuse>
            <specular>0.2 0.8 0.2 1</specular>
          </material>
        </visual>
      </link>
    </model>

    <model name="sky_drone" canonical_link="body">
      <pose>0 0 5 0 0 0</pose>

      <link name="body">
        <pose relative_to="__model__">0 0 0 0 0 0</pose>
        <inertial>
          <mass>2.0</mass>
          <inertia>
            <ixx>0.1</ixx><ixy>0</ixy><ixz>0</ixz>
            <iyy>0.1</iyy><iyz>0</iyz><izz>0.2</izz>
          </inertia>
        </inertial>
        <visual name="visual">
          <geometry>
            <box><size>0.6 0.6 0.2</size></box>
          </geometry>
          <material>
            <ambient>0.0 0.0 0.0 1</ambient>
            <diffuse>0.1 0.1 0.1 1</diffuse>
            <specular>0.8 0.8 0.8 1</specular>
          </material>
        </visual>
        <collision name="collision">
          <geometry>
            <box><size>0.6 0.6 0.2</size></box>
          </geometry>
        </collision>
      </link>

      <link name="arm_fl">
        <pose relative_to="body">0.4 0.4 0 0 0 0</pose>
        <inertial>
          <mass>0.1</mass>
          <inertia><ixx>0.001</ixx><ixy>0</ixy><ixz>0</ixz><iyy>0.001</iyy><iyz>0</iyz><izz>0.001</izz></inertia>
        </inertial>
        <visual name="visual">
          <geometry><cylinder><radius>0.02</radius><length>0.3</length></cylinder></geometry>
          <material><ambient>1.0 0.0 0.0 1</ambient><diffuse>1.0 0.0 0.0 1</diffuse><specular>1.0 0.0 0.0 1</specular></material>
        </visual>
        <collision name="collision">
          <geometry><cylinder><radius>0.02</radius><length>0.3</length></cylinder></geometry>
        </collision>
      </link>

      <link name="arm_fr">
        <pose relative_to="body">0.4 -0.4 0 0 0 0</pose>
        <inertial>
          <mass>0.1</mass>
          <inertia><ixx>0.001</ixx><ixy>0</ixy><ixz>0</ixz><iyy>0.001</iyy><iyz>0</iyz><izz>0.001</izz></inertia>
        </inertial>
        <visual name="visual">
          <geometry><cylinder><radius>0.02</radius><length>0.3</length></cylinder></geometry>
          <material><ambient>0.0 1.0 0.0 1</ambient><diffuse>0.0 1.0 0.0 1</diffuse><specular>0.0 1.0 0.0 1</specular></material>
        </visual>
        <collision name="collision">
          <geometry><cylinder><radius>0.02</radius><length>0.3</length></cylinder></geometry>
        </collision>
      </link>

      <link name="arm_bl">
        <pose relative_to="body">-0.4 0.4 0 0 0 0</pose>
        <inertial>
          <mass>0.1</mass>
          <inertia><ixx>0.001</ixx><ixy>0</ixy><ixz>0</ixz><iyy>0.001</iyy><iyz>0</iyz><izz>0.001</izz></inertia>
        </inertial>
        <visual name="visual">
          <geometry><cylinder><radius>0.02</radius><length>0.3</length></cylinder></geometry>
          <material><ambient>0.0 0.0 1.0 1</ambient><diffuse>0.0 0.0 1.0 1</diffuse><specular>0.0 0.0 1.0 1</specular></material>
        </visual>
        <collision name="collision">
          <geometry><cylinder><radius>0.02</radius><length>0.3</length></cylinder></geometry>
        </collision>
      </link>

      <link name="arm_br">
        <pose relative_to="body">-0.4 -0.4 0 0 0 0</pose>
        <inertial>
          <mass>0.1</mass>
          <inertia><ixx>0.001</ixx><ixy>0</ixy><ixz>0</ixz><iyy>0.001</iyy><iyz>0</iyz><izz>0.001</izz></inertia>
        </inertial>
        <visual name="visual">
          <geometry><cylinder><radius>0.02</radius><length>0.3</length></cylinder></geometry>
          <material><ambient>1.0 1.0 0.0 1</ambient><diffuse>1.0 1.0 0.0 1</diffuse><specular>1.0 1.0 0.0 1</specular></material>
        </visual>
        <collision name="collision">
          <geometry><cylinder><radius>0.02</radius><length>0.3</length></cylinder></geometry>
        </collision>
      </link>

      <link name="rotor_fl">
        <pose relative_to="arm_fl">0 0 0.2 0 0 0</pose>
        <inertial><mass>0.05</mass><inertia><ixx>0.001</ixx><ixy>0</ixy><ixz>0</ixz><iyy>0.001</iyy><iyz>0</iyz><izz>0.001</izz></inertia></inertial>
        <visual name="visual">
          <geometry><cylinder><radius>0.15</radius><length>0.02</length></cylinder></geometry>
          <material><ambient>0.8 0.8 0.8 0.7</ambient><diffuse>0.8 0.8 0.8 0.7</diffuse><specular>0.8 0.8 0.8 0.7</specular></material>
        </visual>
        <collision name="collision">
          <geometry><cylinder><radius>0.15</radius><length>0.02</length></cylinder></geometry>
        </collision>
      </link>

      <link name="rotor_fr">
        <pose relative_to="arm_fr">0 0 0.2 0 0 0</pose>
        <inertial><mass>0.05</mass><inertia><ixx>0.001</ixx><ixy>0</ixy><ixz>0</ixz><iyy>0.001</iyy><iyz>0</iyz><izz>0.001</izz></inertia></inertial>
        <visual name="visual">
          <geometry><cylinder><radius>0.15</radius><length>0.02</length></cylinder></geometry>
          <material><ambient>0.8 0.8 0.8 0.7</ambient><diffuse>0.8 0.8 0.8 0.7</diffuse><specular>0.8 0.8 0.8 0.7</specular></material>
        </visual>
        <collision name="collision">
          <geometry><cylinder><radius>0.15</radius><length>0.02</length></cylinder></geometry>
        </collision>
      </link>

      <link name="rotor_bl">
        <pose relative_to="arm_bl">0 0 0.2 0 0 0</pose>
        <inertial><mass>0.05</mass><inertia><ixx>0.001</ixx><ixy>0</ixy><ixz>0</ixz><iyy>0.001</iyy><iyz>0</iyz><izz>0.001</izz></inertia></inertial>
        <visual name="visual">
          <geometry><cylinder><radius>0.15</radius><length>0.02</length></cylinder></geometry>
          <material><ambient>0.8 0.8 0.8 0.7</ambient><diffuse>0.8 0.8 0.8 0.7</diffuse><specular>0.8 0.8 0.8 0.7</specular></material>
        </visual>
        <collision name="collision">
          <geometry><cylinder><radius>0.15</radius><length>0.02</length></cylinder></geometry>
        </collision>
      </link>

      <link name="rotor_br">
        <pose relative_to="arm_br">0 0 0.2 0 0 0</pose>
        <inertial><mass>0.05</mass><inertia><ixx>0.001</ixx><ixy>0</ixy><ixz>0</ixz><iyy>0.001</iyy><iyz>0</iyz><izz>0.001</izz></inertia></inertial>
        <visual name="visual">
          <geometry><cylinder><radius>0.15</radius><length>0.02</length></cylinder></geometry>
          <material><ambient>0.8 0.8 0.8 0.7</ambient><diffuse>0.8 0.8 0.8 0.7</diffuse><specular>0.8 0.8 0.8 0.7</specular></material>
        </visual>
        <collision name="collision">
          <geometry><cylinder><radius>0.15</radius><length>0.02</length></cylinder></geometry>
        </collision>
      </link>

      <joint name="arm_fl_joint" type="fixed">
        <parent>body</parent>
        <child>arm_fl</child>
      </joint>
      <joint name="arm_fr_joint" type="fixed">
        <parent>body</parent>
        <child>arm_fr</child>
      </joint>
      <joint name="arm_bl_joint" type="fixed">
        <parent>body</parent>
        <child>arm_bl</child>
      </joint>
      <joint name="arm_br_joint" type="fixed">
        <parent>body</parent>
        <child>arm_br</child>
      </joint>

      <joint name="rotor_fl_joint" type="revolute">
        <parent>arm_fl</parent>
        <child>rotor_fl</child>
        <axis>
          <xyz>0 0 1</xyz>
          <limit>
            <lower>-1.79769e+308</lower>
            <upper>1.79769e+308</upper>
          </limit>
        </axis>
      </joint>
      <joint name="rotor_fr_joint" type="revolute">
        <parent>arm_fr</parent>
        <child>rotor_fr</child>
        <axis>
          <xyz>0 0 1</xyz>
          <limit>
            <lower>-1.79769e+308</lower>
            <upper>1.79769e+308</upper>
          </limit>
        </axis>
      </joint>
      <joint name="rotor_bl_joint" type="revolute">
        <parent>arm_bl</parent>
        <child>rotor_bl</child>
        <axis>
          <xyz>0 0 1</xyz>
          <limit>
            <lower>-1.79769e+308</lower>
            <upper>1.79769e+308</upper>
          </limit>
        </axis>
      </joint>
      <joint name="rotor_br_joint" type="revolute">
        <parent>arm_br</parent>
        <child>rotor_br</child>
        <axis>
          <xyz>0 0 1</xyz>
          <limit>
            <lower>-1.79769e+308</lower>
            <upper>1.79769e+308</upper>
          </limit>
        </axis>
      </joint>

      <plugin filename="libignition-gazebo-joint-controller-system.so"
              name="ignition::gazebo::systems::JointController">
        <joint_name>rotor_fl_joint</joint_name>
      </plugin>
      <plugin filename="libignition-gazebo-joint-controller-system.so"
              name="ignition::gazebo::systems::JointController">
        <joint_name>rotor_fr_joint</joint_name>
      </plugin>
      <plugin filename="libignition-gazebo-joint-controller-system.so"
              name="ignition::gazebo::systems::JointController">
        <joint_name>rotor_bl_joint</joint_name>
      </plugin>
      <plugin filename="libignition-gazebo-joint-controller-system.so"
              name="ignition::gazebo::systems::JointController">
        <joint_name>rotor_br_joint</joint_name>
      </plugin>
    </model>

      <!-- 上升 (上箭头) -->
    <plugin filename="libignition-gazebo-triggered-publisher-system.so"
            name="ignition::gazebo::systems::TriggeredPublisher">
      <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
        <match>data: 16777235</match>
      </input>
      <output type="ignition.msgs.EntityWrench" topic="/world/sky_world/wrench">
        entity { name: "sky_drone::body" type: LINK }
        wrench { force { z: 2000.0 } }
      </output>
    </plugin>

    <!-- 下降 (下箭头) -->
    <plugin filename="libignition-gazebo-triggered-publisher-system.so"
            name="ignition::gazebo::systems::TriggeredPublisher">
      <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
        <match>data: 16777237</match>
      </input>
      <output type="ignition.msgs.EntityWrench" topic="/world/sky_world/wrench">
        entity { name: "sky_drone::body" type: LINK }
        wrench { force { z: -5.0 } }
      </output>
    </plugin>

    <!-- 前进 (W) -->
    <plugin filename="libignition-gazebo-triggered-publisher-system.so"
            name="ignition::gazebo::systems::TriggeredPublisher">
      <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
        <match>data: 87</match>
      </input>
      <output type="ignition.msgs.EntityWrench" topic="/world/sky_world/wrench">
        entity { name: "sky_drone::body" type: LINK }
        wrench { force { x: 150.0 } }
      </output>
    </plugin>

    <!-- 后退 (S) -->
    <plugin filename="libignition-gazebo-triggered-publisher-system.so"
            name="ignition::gazebo::systems::TriggeredPublisher">
      <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
        <match>data: 83</match>
      </input>
      <output type="ignition.msgs.EntityWrench" topic="/world/sky_world/wrench">
        entity { name: "sky_drone::body" type: LINK }
        wrench { force { x: -150.0 } }
      </output>
    </plugin>

    <!-- 左移 (A) -->
    <plugin filename="libignition-gazebo-triggered-publisher-system.so"
            name="ignition::gazebo::systems::TriggeredPublisher">
      <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
        <match>data: 65</match>
      </input>
      <output type="ignition.msgs.EntityWrench" topic="/world/sky_world/wrench">
        entity { name: "sky_drone::body" type: LINK }
        wrench { force { y: 150.0 } }
      </output>
    </plugin>

    <!-- 右移 (D) -->
    <plugin filename="libignition-gazebo-triggered-publisher-system.so"
            name="ignition::gazebo::systems::TriggeredPublisher">
      <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
        <match>data: 68</match>
      </input>
      <output type="ignition.msgs.EntityWrench" topic="/world/sky_world/wrench">
        entity { name: "sky_drone::body" type: LINK }
        wrench { force { y: -150.0 } }
      </output>
    </plugin>

    <!-- 左转 (左箭头) -->
    <plugin filename="libignition-gazebo-triggered-publisher-system.so"
            name="ignition::gazebo::systems::TriggeredPublisher">
      <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
        <match>data: 16777234</match>
      </input>
      <output type="ignition.msgs.EntityWrench" topic="/world/sky_world/wrench">
        entity { name: "sky_drone::body" type: LINK }
        wrench { torque { z: 1500.0 } }
      </output>
    </plugin>

    <!-- 右转 (右箭头) -->
    <plugin filename="libignition-gazebo-triggered-publisher-system.so"
            name="ignition::gazebo::systems::TriggeredPublisher">
      <input type="ignition.msgs.Int32" topic="/keyboard/keypress">
        <match>data: 16777236</match>
      </input>
      <output type="ignition.msgs.EntityWrench" topic="/world/sky_world/wrench">
        entity { name: "sky_drone::body" type: LINK }
        wrench { torque { z: -1500.0 } }
      </output>
    </plugin>

  </world>
</sdf>

## 重要事项

1. ApplyLinkWrench is a world-level plugin — it must be placed directly under <world>, not inside a <model>.


## gazebo常用命令及解释

1. ign gazebo -v 4 dji_m350.sdf -s.

  对于我的fortress，记住，只能用这个ign gazebo。

  这句话不开gui，调试sdf。

2. ign gazebo -v 4 djim350.sdf.

  开始仿真。