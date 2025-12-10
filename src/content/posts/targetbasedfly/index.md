---
title: target_based_fly
published: 2026-01-01T01:57:50.096Z
---


有个类叫 TargetBasedModeBase，继承自 ModeBase（PX4 ROS 2 的自定义飞行模式基类）。

trajectory_setpoint_ = std::shared_ptr<px4_ros2::TrajectorySetpointType>(
    new px4_ros2::TrajectorySetpointType(*this));





“我要控制摄像头抓拍，于是我在节点里创建了一个服务客户端，指向 /set_capture，以后想拍照就用它发请求。”

在 ROS 2 里,服务（Service）分为 服务端（Server） 和 客户端（Client）。

px4_ros2::OdometryLocalPosition 是 PX4 ROS 2 接口里封装的一个类，用来获取无人机的本地位置状态（Odometry）。

它是 无人机在局部坐标系下的“GPS+IMU融合状态” 的封装。

namespace {
constexpr char kSetCaptureService[] = "/set_capture";
}  // namespace

匿名命名空间中的内容 只能在当前 .cpp 文件内访问。

set_capture_client_ = node_.create_client<std_srvs::srv::SetBool>(kSetCaptureService);

kSetCaptureService 就是服务的名字 /set_capture。客户端通过这个名字调用对应的 ROS2 服务。

前缀 k 表示 常量（Google C++ Style Guide 推荐）。

ros2 service list | grep set_capture.

NodeWithModeExecutor 本身就是一个模板类（class template）。

create_publisher 是 父类（rclcpp::Node）的方法。

px4_msgs::msg::VehicleCommand 是 PX4 Autopilot 提供的一种 ROS 2 消息类型，用于发送飞机/多旋翼等飞行器的控制指令。

这个消息定义来自 PX4 ROS 消息包（px4_msgs），目的是在 ROS 2 节点和 PX4 飞控之间通信。

RCLCPP_INFO(this->get_logger(), "Land mode service created at: activate_land_mode");

this → 指向当前对象。

MAVLink 里有 Storm32 的 XML ,怎么理解？？？

message_definitions/v1.0/storm32.xml.

MAVLink 的消息/命令都是用 XML 文件定义 的。

message_definitions/v1.0/all.xml这个文件干啥的？？？

直接在 RGB 上做颜色过滤很麻烦？？？

HSV（色调 Hue, 饱和度 Saturation, 明度 Value）更适合分辨颜色？？？

红色在 RGB 中是 R 很大，G、B 很小。

明亮：红色可能变成 (255, 20, 20)。

昏暗：红色可能变成 (100, 5, 5)。

OpenCV 的 HSV 色调 H 不是 0–360，而是 0–180（整型值）。

Hue 在 HSV 是环形的（色环）。

饱和度 S（Saturation），范围：0–255。

cv的 inRange 会帮助生成 mask，把满足这个条件的像素标记成 255，其它都变 0 。

PX4 的 NED 本地坐标系 是：

以起飞点为原点，用 北–东–下（North–East–Down） 定义的三维直角坐标系，用来表示飞机在“附近空间”的位置和速度。


## 开发笔记

1. 我想用ros2来开发一个项目，同时希望协同开发，也就是用上github,那我是把ws开源还是把包开源？反正这个项目的目的就是，机载电脑运行ros2然后通过摄像头捕获一个码，然后opencv计算位姿，给px4飞控引导来实现精确起飞和着陆。

    Workspace (ws)，这是ROS2的开发根目录，通常包含src/文件夹（里面放packages）、build/、install/和log/等。workspace是用来构建和运行的"容器"，但在版本控制中，不应该把整个ws都push到GitHub，尤其是build/install这些生成的文件夹（它们是临时文件，会因环境不同而变）。

    对于协同开发（多人用GitHub协作），建议开源整个项目作为一个GitHub仓库（repo），但只版本控制workspace的核心部分，而不是把build/install等push上去。

2. sudo apt install libopencv-dev 这个命令的作用是？？？

    让 CMake 能轻松找到 OpenCV，find_package(OpenCV REQUIRED) 就能用了。

3. 环境介绍。

    ubuntu22笔记本用作开发。

    ubuntu22香橙派（RK3588）作为机载电脑。

    PX4飞控（1.17.0）。

4. apt install ros-humble-image-transport-plugins.

5. sudo apt install ros-humble-rqt-image-view

source /opt/ros/humble/setup.bash && source install/setup.bash && ros2 service call /activate_takeoff_mode std_srvs/srv/Empty
waiting for service to become available...

