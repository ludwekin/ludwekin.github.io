---
title: ros2
published: 2025-12-24T06:33:53.396Z
---



vcs 是 Version Control System 的缩写，是 ROS 里常用来批量管理源码仓库的工具（类似 git / svn 集成工具）。

vcs import third < circle_pilot.repos。

vcs import <target_dir>，作用是把 .repos 文件里列出的仓库导入（克隆）到目标目录。

third，这里是目标目录，相当于把所有仓库克隆到 third/ 文件夹里。

< circle_pilot.repos，表示 从文件 circle_pilot.repos 读取仓库列表。

mkdir -p ~/ws/src，现在就有了 ROS 2 的“项目根目录”。





colcon 是 ROS 2 官方推荐的构建工具，用来构建 ROS 2 工作空间。

source ~/circle_pilot/install/setup.zsh。


PX4 SITL 默认会把数据发到 UDP 14540，所以 mavlink-router 监听该端口，然后转发到 Orange Pi 的 14540。


sensor_msgs/Image 是 ROS（Robot Operating System，机器人操作系统） 中的一个标准消息类型，属于 sensor_msgs 包。它用于传输未压缩的图像数据（uncompressed image），常用于摄像头（如USB相机、深度相机等）发布的图像话题（topic），例如 /camera/image_raw 或 /usb_cam/image_raw。

与之对应的压缩版本是 sensor_msgs/CompressedImage（用于节省带宽）。

ros2 node list???


ros2 topic list和ros2 node list区别？

节点是 ROS 2 中的执行单元，每个节点可以发布（publish）或订阅（subscribe）Topic，也可以提供（service）或调用（client）服务。

Topic 是 ROS 2 中节点之间通信的通道，数据通过 Topic 流动。

ros2 topic info /chatter。



RCLCPP_INFO 是 rclcpp 提供的日志宏，用于输出 INFO 级别的日志消息（类似于 printf，但更安全且支持流式输出）。

hardcode的landing_target_detector.yaml等等是怎么在运行sprite 10 1a的时候加载的？

默认路径 ~/.circle_pilot/config/landing_target_detector.yaml 的定义位置？





---
title: ros2
published: 2025-12-24T06:33:53.396Z
---



vcs 是 Version Control System 的缩写，是 ROS 里常用来批量管理源码仓库的工具（类似 git / svn 集成工具）。

vcs import third < circle_pilot.repos。

vcs import <target_dir>，作用是把 .repos 文件里列出的仓库导入（克隆）到目标目录。

third，这里是目标目录，相当于把所有仓库克隆到 third/ 文件夹里。

< circle_pilot.repos，表示 从文件 circle_pilot.repos 读取仓库列表。

mkdir -p ~/ws/src，现在就有了 ROS 2 的“项目根目录”。





colcon 是 ROS 2 官方推荐的构建工具，用来构建 ROS 2 工作空间。

source ~/circle_pilot/install/setup.zsh。


PX4 SITL 默认会把数据发到 UDP 14540，所以 mavlink-router 监听该端口，然后转发到 Orange Pi 的 14540。


sensor_msgs/Image 是 ROS（Robot Operating System，机器人操作系统） 中的一个标准消息类型，属于 sensor_msgs 包。它用于传输未压缩的图像数据（uncompressed image），常用于摄像头（如USB相机、深度相机等）发布的图像话题（topic），例如 /camera/image_raw 或 /usb_cam/image_raw。

与之对应的压缩版本是 sensor_msgs/CompressedImage（用于节省带宽）。

ros2 node list???


ros2 topic list和ros2 node list区别？

节点是 ROS 2 中的执行单元，每个节点可以发布（publish）或订阅（subscribe）Topic，也可以提供（service）或调用（client）服务。

Topic 是 ROS 2 中节点之间通信的通道，数据通过 Topic 流动。

ros2 topic info /chatter。



RCLCPP_INFO 是 rclcpp 提供的日志宏，用于输出 INFO 级别的日志消息（类似于 printf，但更安全且支持流式输出）。

hardcode的landing_target_detector.yaml等等是怎么在运行sprite 10 1a的时候加载的？

默认路径 ~/.circle_pilot/config/landing_target_detector.yaml 的定义位置？


1. colcon build --packages-select pkg1 pkg2 pkg3.

    这个命令是只构建指定的包。速度快一点。

2. 关于ros2的ws概念，拿circle pilot这个项目来说，就是在ws下的src里面写circle pilot。

3. source install/setup.bash.

    把你刚刚用 colcon build 编译出来的 workspace（工作空间）里的所有包、节点、可执行文件、库、Python模块、环境变量等，全部“登记”到当前终端里，让系统知道它们的存在。

4. ROS 2 的“包” ≈ 一个独立的小工程 / 小模块 / 小功能单元。


    把 ROS 2 比作一个超级大的乐高玩具城，整个 workspace ≈ 一个大玩具箱。

    “包”就是 ROS 2 里最小的、可独立编译、可独立运行、可被别人依赖的“功能积木”。

5. .msg 文件就是 ROS（包括 ROS 1 和 ROS 2）里用来定义“消息”（Message）的文件。

    它是你自己定义的一种数据结构，用来让不同的节点（程序）之间通过话题（Topic）互相传递数据。

6. 常见的包。

    xxx_bringup，启动整个机器人最基础的东西（launch 文件）。

    xxx_hardware，，硬件驱动、和真实电机/传感器通信。

7. colcon build 是在干嘛？

    colcon build 是 ROS 2 的新一代构建工具（取代了 ROS 1 的 catkin_make）。

    对每个包依次执行cmake 配置（C++ 包），编译，然后安装到 install/ 目录。

    同时还会生成 build/、log/ 目录。

8. colcon 决定包编译顺序的主要依据。

    package.xml 里写了明确的依赖关系。

    同一个包里 C++ 和 Python 的内部依赖。colcon 会先处理 CMake（C++ 部分），再处理 Python 部分。

9. uXRCE-DDS 中间件由两部分组成，客户端（Client），运行在 PX4 固件内部。代理（Agent），运行在伴侣计算机上。

    两者之间通过串口或 UDP 进行双向数据交换。

10. PX4 的 uxrce_dds_client 模块是在编译固件时自动生成的。编译时会参考配置文件 src/modules/uxrce_dds_client/dds_topics.yaml，只把 yaml 里列出的那部分消息编译进 uxrce_dds_client 模块。




