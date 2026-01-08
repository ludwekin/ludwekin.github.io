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




