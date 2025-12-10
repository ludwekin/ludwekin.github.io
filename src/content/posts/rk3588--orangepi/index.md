---
title: rk3588 & orangepi
published: 2025-12-24T03:20:11.004Z
---

香橙派接到了公司网络。

ssh orangepi@192.168.124.57。

开发板Orange Pi 5 Max，目前为止官网显示的操作系统只支持到Ubuntu 22.04。

所以系统我们装的就是Ubuntu 22.04。

香橙派的root密码是orangepi。

ROS 2 Humble 只支持 Ubuntu 22.04 (jammy)。

ros-humble-v4l2-camera。


ros-humble-aruco-opencv。


ros-humble-cv-bridge。


ros-humble-ros-gz-bridge。



ffplay -f video4linux2 -input_format mjpeg -video_size 1280x1024 -framerate 1 -i /dev/video0.

libGL error: failed to load driver: rockchip (and dri3/dri2 failures)
This is a common issue on Rockchip RK3588-based boards like the Orange Pi 5 Max when using Mesa/OpenGL without proper GPU driver configuration (Panfrost/Panthor for Mali-G610). FFplay falls back to software rendering (LLVMpipe), which works but is slower and triggers these errors. It doesn't prevent playback, but hardware acceleration is missing.
For better performance, check your distro's docs (e.g., Armbian or official Orange Pi images often need extra steps for Mali GPU drivers).

ps -aux | grep ffplay。

v4l2-ctl --list-formats-ext -d /dev/video0。

source /opt/ros/humble/s
etup.bash



ros2 launch circle_pilot_bringup sprite_10_a1_takeoff_and_land.launch.py如果执行这个命令。起飞文件是什么？

查看该 launch 文件，确定它启动的起飞文件。
查看起飞执行器的 launch 文件，确认启动的节点：
查看起飞执行器的主程序文件：
执行该命令时，起飞相关文件如下：
起飞相关文件
Launch 文件：
circle_pilot_takeoff_and_land/launch/target_based_takeoff_executor.launch.py
由主 launch 文件（第 151-167 行）包含启动
可执行节点：
target_based_takeoff_executor（由源代码编译生成）
源代码文件：
circle_pilot_takeoff_and_land/src/target_based_takeoff_executor_main.cpp - 主程序入口
circle_pilot_takeoff_and_land/include/circle_pilot_takeoff_and_land/target_based_takeoff_executor.hpp - 执行器实现
circle_pilot_takeoff_and_land/include/circle_pilot_takeoff_and_land/target_based_takeoff.hpp - 起飞模式定义
circle_pilot_takeoff_and_land/src/target_based_takeoff.cpp - 起飞模式实现
配置文件：
target_based_takeoff.yaml（默认在 ~/.circle_pilot/config/ 目录，或使用 takeoff_config_file 参数指定）
核心起飞逻辑在 target_based_takeoff_executor.hpp 和 target_based_takeoff.cpp 中实现。

ros-humble-ros-gz-bridge。