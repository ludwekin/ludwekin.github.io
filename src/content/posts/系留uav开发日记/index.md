---
title: 系留uav开发记载
published: 2025-12-17T02:17:34.462Z
draft: true
---


# 纵览 

项目部署到了gitea私有仓库。

可以随时把已经 git add 的文件“撤下来”。

重新熟悉一下，基于一个branch开发，然后作出代码改动，输入git checkout -b feature/aruco-takeoff-landing来切换到我要的新分支。

git commit -m "Integrate Gazebo ArUco takeoff and landing apps"。

git push -u origin feature/aruco-takeoff-landing。完成。

cvitek的依赖在么装来着？？




# 日记：
19号，调试机载电脑rk3588。真机测试。试飞出现重大问题，代码整改。要重新跑仿真。
18号，上午请假了。下午融合自动起飞逻辑到uvc程序中。调试uvc摄像头。测试跨机器仿真（mavlink转发）。合并分支。
17号，开发新特性，提交新分支。
16号，测试新飞机。切换机载电脑摄像头方向。切换遥控器为sbus协议。研究代码库。成功运行aruco landing仿真。实现gazebo,qgc,cpp app协同仿真。
15号，重新测试cvitek机载电脑版飞机。更新/降级遥控器协议为elrs3.3.6。晚上构建了本地开发。
14号，测试/调试cvitek机载电脑版飞机。交叉编译risk-v代码。失败。新电脑改配置。安装必要开发工具/软件/环境。
13号，安装代码库通用依赖。





# Workflow:
  1. Start PX4 SITL: make px4_sitl gz_x500_mono_cam_down
  2. Run this app with appropriate Gazebo topic
  3. Trigger precision landing: commander mode auto。
  
  



    



# 常用代码：




cmake --preset x86_64          # 用配置生成构建文件
cmake --build --preset x86_64-release   # 构建

./build/aruco_landing_app \x86-64 --config aruco_landing_config.xml \ --image gazebo_camera_frame.jpg \ --debug

make px4_sitl gz_x500。

./build/x86_64/samples/aruco_landing_app --config config/aruco_landing_config.xml --image <你的图片>.jpg --debug。



./aruco_landing_app --config aruco_landing_config.xml --image 7x7_1000-6.jpg --debug。
 
 
 
./build/x86_64/samples/aruco_landing_app --config config/aruco_landing_config.xml --image assets/singlemarkersoriginal.jpg --debug


./aruco_landing_app --config aruco_landing_config.xml --image iamgekk.jpg --debug。

./gazebo_aruco_landing_app \
    --topic /world/default/model/x500_mono_cam_down_0/link/camera_link/sensor/camera/image \
    --config config/aruco_landing_config.xml
    

这是我写的代码。仿真环境中自动起飞。
./gazebo_aruco_auto_takeoff_app \
    --topic /world/default/model/x500_mono_cam_down_0/link/camera_link/sensor/camera/image \
    --config config/aruco_landing_config.xml


./gazebo_aruco_landing_app \
    --topic /world/default/model/x500_mono_cam_down_0/link/camera_link/sensor/camera/image \
    --config aruco_landing_config。

make px4_sitl gz_x500_mono_cam_down。

make px4_sitl gz_x500_mono_cam_down gazebo_world:=aruco。

错误用法，好像过时了。

make px4_sitl gz_x500_mono_cam_down \
gazebo_world:=Tools/simulation/gz/worlds/aruco.sdf。
  
好像也不对。
  
重要！！！

export PX4_GZ_WORLD=aruco
make px4_sitl gz_x500_mono_cam_down。





./gazebo_aruco_landing_app \
    --topic /world/default/model/x500_mono_cam_down_0/link/camera_link/sensor/camera/image \
    --config aruco_landing_config.xml。
    
./gazebo_aruco_landing_app \
  --topic /world/aruco/model/x500_mono_cam_down_0/link/camera_link/sensor/imager/image \
  --config aruco_landing_config.xml。





./gazebo_aruco_landing_app \
  --topic /world/aruco/model/x500_mono_cam_down_0/link/camera_link/sensor/imager/image \
  --config aruco_landing_config.xml。
  
  这个可以用。



./build/x86_64/samples/gazebo_aruco_landing_profile_app \
  --topic /world/aruco/model/x500_mono_cam_down_0/link/camera_link/sensor/camera/image \
  --config aruco_landing_config.xml





./build/x86_64/samples/gazebo_aruco_landing_profile_app \
  --topic /world/aruco/model/x500_mono_cam_down_0/link/camera_link/sensor/imager/image \
  --config /home/uni/Desktop/drone_toolbox/config/aruco_landing_config.xml
  





# 通用文档



可以切换飞控模式为 Precision Land 入手精准着陆。

在 MAVLink 控制台执行：commander mode auto:precland。

这会让 PX4 进入精准着陆模式，并 默认是“Required（必需）”模式（即必须执行精降）。

安装仿真。

安装java17,因为仿真似乎需要java。

遇到一个问题，官方推荐安装新款gazebo。

同时，ubuntu22默认已经装好了gazebo。


cd /data/open-src/PX4-Autopilot。这个是装好gazebo的px4。

cd /home/uni/Downloads,然后./QGroundControl-x86_64.AppImage。

进了仿真之后，也打开了qgc，然后怎么用遥控器控制呢？


仿真时，PX4 默认通过 mavlink 与 QGC 通信。

cp /home/uni/Desktop/drone_toolbox/models/worlds/ship_cabin.sdf /data/open-src/PX4-Autopilot/Tools/simulation/gz/worlds/。

cp /home/uni/Desktop/drone_toolbox/models/worlds/balloon_world.sdf /data/open-src/PX4-Autopilot/Tools/simulation/gz/worlds/。



cd /home/uni/Downloads, ./QGroundControl-x86_64.AppImage。




if (proc_result.markers_detected > 0) {
    if (proc_result.target_marker_id.has_value()) {
        char buf[512];
        snprintf(buf, sizeof(buf),
                 "TARGET: marker_body=(%.2f,%.2f,%.2f)m | "
                 "drone_NED=(%.2f,%.2f,%.2f)m%s",
                 proc_result.target_position_x_m,
                 proc_result.target_position_y_m,
                 proc_result.target_position_z_m,
                 pos.position.north_m,
                 pos.position.east_m,
                 pos.position.down_m,
                 proc_result.landing_target_published ? " [SENT]" : "");
        logger.information(std::string(buf));
    }
}


为什么我们在 DuoS RISC-V 开发板上就能直接跑这个 aruco_landing_mpi_app？？？

DuoS 上你跑的是 Companion Computer（地面计算机）模式，通过串口或者 UDP/TCP 连接 PX4 飞控，发送 MAVLink 消息。

只要串口或者网络能通，MAVSDK 就能工作。

只要 OpenCV 和 Eigen 都能在 RISC-V 上跑，ArUco 检测就能正常执行。


MavsdkSubsystem 是一个 Poco Framework 的子系统（Subsystem），用于管理 MAVSDK 连接。class MavsdkSubsystem : public Poco::Util::Subsystem {。

核心成员变量：
private:  std::string connection_uri_;  std::unique_ptr<mavsdk::Mavsdk> mavsdk_;

ArUco 精确着陆系统使用计算机视觉技术，通过检测地面上的 ArUco 标记来实现无人机的高精度自主着陆。系统通过相机拍摄地面图像，检测并识别 ArUco 标记，估算标记的 6 自由度位姿（3D 位置 + 3D 姿态），并将着陆目标信息发送给飞控系统（PX4/ArduPilot）。

这里的估算标记的 6 自由度位姿（3D 位置 + 3D 姿态）怎么理解？

OpenCV ArUco 预定义字典（4x4 ， 7x7，以及 AprilTag），这是什么？

在 OpenCV 的 aruco 模块里，有一批官方内置的字典（dictionary），你不用自己生成码表，直接用即可。

「4x4 到 7x7」是什么意思？

4x4，每个码是 4×4 的比特矩阵。远距离可识别性上，4x4 > 7x7 。

DICT_5X5_50，后面的 50 表示，这个字典里一共有多少个不同的 marker ID。

而AprilTag 是另一套标记体系。

Duo S 跑的就是 Linux（RISC-V 64 位，riscv64）。SoC是SG2002 / SG2000 系列。

算能（SOPHGO，算能科技）是一家中国芯片公司。算能芯片工具链一般是riscv64-linux-gnu。

Duo S 用的 SoC，是算能设计的。

rk3588跑px4机载电脑的性能如何？

aruco_landing_mpi_app，讲讲duos运行这个elf文件后，底层是怎么运行的，怎么链接的？


内存布局（运行时）：

ELF 文件加载到内存：
┌─────────────────────┐
│ 代码段 (.text)      │ ← 所有静态链接的代码
│ 数据段 (.data)      │ ← 全局变量
│ BSS 段 (.bss)       │ ← 未初始化数据
│ 堆 (heap)           │ ← malloc/new 分配
│ 栈 (stack)          │ ← 局部变量、函数调用
└─────────────────────┘

VB Pool（视频缓冲区）：
┌─────────────────────┐
│ VB Pool (内核空间)  │ ← CVitek 驱动管理
│ DMA 缓冲区          │ ← 传感器数据直接写入
└─────────────────────┘




RK3588（ARM64），主流 Linux/Ubuntu/Yocto 支持良好，能直接运行 ROS2、OpenCV。

RK3588 是一颗 SoC（芯片）。


OpenCV = 用 C++ 写的计算机视觉库，C++ 调用是最完整、性能最好的方式。

RK3588为什么能运行arm64的ubuntu？和我们家用电脑的x86_64的ubuntu有区别吗？

RK3588 的 CPU 是 ARM64，和 Apple M1 / Jetson / 树莓派 4 是同一“语言体系”。

ROS / MAVSDK 在 RK3588 上可能还挺合适。直接 apt 装就能跑。

Linter（静态代码检查器） 是一种工具，用来在不运行代码的情况下检查码风格是否规范。  



/world/aruco/model/x500_mono_cam_down_0/link/camera_link/sensor/imager/image这个是相机话题。

gz topic -e -t /world/aruco/model/x500_mono_cam_down_0/link/camera_link/sensor/imager/image，检查话题是否在发布数据。

./build/x86_64/samples/gazebo_aruco_landing_profile_app \
  --topic /world/aruco/model/x500_mono_cam_down_0/link/camera_link/sensor/imager/image \
  --config config/aruco_landing_gazebo.xml \
  --connection udp://:14540

这个指令是对的。

AUTO.LAND 是合法入口。

cd /home/uni/Desktop/drone_toolbox && cmake --build --preset x86_64-release --target gazebo_aruco_landing_profile_app

理解这个代码。

只有低于一定高度，才会进入 Precision Landing。

参数PLD_MAX_ALT   默认 10 m。

pxh> listener vehicle_status。

刘工的代码里面，precision landing 是可以通过 "commander mode auto:precland" 来触发的？？

卢老板也是这个意思。

我的程序中，我的无人机需要飞到40米定高，这一步是怎么实现的？是靠position模式吗？还是靠hold??现在的40米定高40s是靠的offboard。


首先先研究能不能手动切换成precision landing。

在qgc里面可以点击precision landing也可以通过mavlink console来切换，但是奇怪的是居然不停到码上了。


precision landing逻辑完全不对，仿真的时候设置搜索高度为100米，结果只飞到40米就降落了。

理解Merge pull request 。


./build/x86_64/samples/aruco_landing_and_auto_takeoff \
  --topic /world/aruco/model/x500_mono_cam_down_0/link/camera_link/sensor/imager/image \
  --config config/aruco_landing_gazebo.xml \
  --connection udp://:14540

别人如何实现的precision landing？

make px4_sitl gz_x500_mono_cam_down_aruco也能用。


仿真不落到aruco码可以理解，因为没有LANDING_TARGET消息？


pxh> listener landing_target_pose

TOPIC: landing_target_pose
 landing_target_pose
    timestamp: 911976000 (345.795990 seconds ago)
    x_rel: 0.00000
    y_rel: 0.00000
    z_rel: 0.00000
    vx_rel: 0.00000
    vy_rel: 0.00000
    cov_x_rel: 0.00000
    cov_y_rel: 0.00000
    cov_vx_rel: 0.00000
    cov_vy_rel: 0.00000
    x_abs: 9.23276
    y_abs: 15.15823
    z_abs: 1.37654
    is_static: False
    rel_pos_valid: False
    rel_vel_valid: False
    abs_pos_valid: True

这个消息好像是对的。老板的返回结果也是这个。

检查px4飞行日志。




另外一个核心是 gazebo_aruco_landing_profile_app.cc 中 从 2 m → 40 m → 保持 40 m 的高度 profile 是如何在 PX4 的 Offboard 模式下实现的？

我想修改px4固件，然后编译到holybro的6c mini，可行吗？

对了，飞机在40米的时候，offboard一直要发绝对位置来保持高度，但是可以用position模式就可以直接保持高度。

gz_bridge 是 PX4 和 Gazebo 的接口。

当目标丢失超过 _param_pld_srch_tout 时间，状态机进入 Search。

无人机爬升到搜索高度 _param_pld_srch_alt，循环等待目标重新出现。超时则进入 Fallback。

mavlink控制台例子：param set PLD_BTOUT 5 。

git config --global user.email "$NEW_EMAIL".

git config --global user.name  "benjamin".

git config --global user.email "benjamin.wang@circleai.tech".

Git Commit Message 的常用格式与最佳实践。

比如 feat(aruco): add take‑off command 。

或者 feat: add aruco take‑off command 。

或者 feat(aruco): add take‑off and landing commands

The drone can now detect an ArUco marker on the ground,
estimate its pose and use it as a landing target.
Implemented a new state machine:
  - TAKEOFF → SEARCH_MARKER → LAND
Added unit tests for the pose estimator.

Closes #42
BREAKING CHANGE: The `DroneController.start()` API now returns a Promise.

上面的 Footer 用来关联 Issue。

git push 是把 本地提交推送到远程仓库，通常不需要填写消息。

每个 commit 应该代表 一个独立的、可理解的修改。

分批commit最佳实例：

git add file1 file2
git commit -m "第一个功能/修改的描述"

git add file3
git commit -m "第二个功能/修改的描述"

在 Git 提交消息里，除了 feat（新功能）之外，社区里常用 Conventional Commits 的类型有很多。

feat，fix，docs，chore，perf。

commit消息最佳实例：

feat(auth): 添加用户登录功能

支持邮箱和手机号登录，并增加密码强度校验

system 是一个 MAVSDK 的系统对象，表示一架无人机。

用 shared_ptr 管理它的生命周期，保证在 LandingTargetPublisher 使用时系统对象不会被提前释放。

LandingTargetPublisher::LandingTargetPublisher(
    std::shared_ptr<mavsdk::System> system)
    : system_(system), update_rate_hz_(10.0), message_count_(0) {
  if (!system_) {
    Poco::Logger& logger = Poco::Logger::get("LandingTargetPublisher");
    logger.error("Invalid MAVSDK system provided");
    return;
  }

理解？

构造函数需要传入一个 System 智能指针。System 就是无人机的接口，类里的函数会通过它发送 MAVLink 消息。

: system_(system), update_rate_hz_(10.0), message_count_(0),初始化列表。

Poco::Logger& logger = Poco::Logger::get("LandingTargetPublisher");理解？

声明一个 Logger 类的引用变量 logger。Poco 是 命名空间（namespace），类似一个目录，用于组织类和函数。

引用意味着 logger 并不是新建一个 Logger 对象，而是指向现有对象。

Poco::Logger::get 是 Logger 类的静态成员函数（static method）。静态函数可以直接通过类名调用，而不需要先创建对象。get 方法通常用于 获取 Logger 实例。返回值是 Logger& 类型，也就是 Logger 的引用。

整行代码意思就是，声明一个 Logger 的引用 logger，并指向 Poco 日志库中名为 "LandingTargetPublisher" 的日志实例，以便在代码中使用 logger 输出日志。

“线程安全”确实是 Poco::Logger 的一个重要特点。

线程安全（thread-safe），指的是：在多线程环境下同时调用同一个对象的方法，不会出现数据竞态（race condition）或崩溃。

对于 Poco::Logger 来说，多个线程同时调用：logger.information("Thread A info")，不会出现日志内容互相覆盖。因为 Logger 内部会处理 互斥锁（mutex） 或其他同步机制，确保日志写入是安全的。


return LandingTarget(marker_ned_absolute, marker_drone.timestamp_us);理解？

return LandingTarget(...) 是 直接构造临时对象并返回。return 把这个临时对象 返回给调用者。

CameraDroneTransform::CameraDroneTransform(const CameraMount& camera_mount)
    : camera_mount_(camera_mount) {
  camera_to_drone_rotation_ = computeCameraToDroneRotation();

  Poco::Logger& logger = Poco::Logger::get("CameraDroneTransform");
  // debug output camera_to_drone_rotation_
  logger.debug("Camera to drone rotation matrix:" +
               FormatTool::formatMatrix3x3(camera_to_drone_rotation_));
  logger.information("Camera-drone transform initialized");
  logger.information("  Camera position: [" +
                     std::to_string(camera_mount_.position.x()) + ", " +
                     std::to_string(camera_mount_.position.y()) + ", " +
                     std::to_string(camera_mount_.position.z()) + "]");
  logger.information("  Camera orientation (RPY): [" +
                     std::to_string(camera_mount_.orientation.x()) + ", " +
                     std::to_string(camera_mount_.orientation.y()) + ", " +
                     std::to_string(camera_mount_.orientation.z()) + "]");
}

这里的 :: 出现了两个，是 C++ 的作用域解析符（scope resolution operator）。表示“这个函数属于某个类”。

左边 CameraDroneTransform:: 表示 这个构造函数属于 CameraDroneTransform 类。

const CameraMount& camera_mount 是传入的参数。表示相机的安装信息（位置 + 方向）。

两个 :: 是 作用域解析符，告诉编译器这是 CameraDroneTransform 类里的函数。这里的 camera_mount_ 是类成员变量。通过初始化列表把传入的 camera_mount 赋值给成员变量 camera_mount_。初始化列表比在构造函数体内赋值更高效，特别是对于类对象。调用类内函数 computeCameraToDroneRotation() 计算从相机坐标系到无人机坐标系的旋转矩阵，并存储在成员变量 camera_to_drone_rotation_ 中。这个旋转矩阵通常是 3x3 矩阵，用于坐标变换。

Poco::Logger& logger = Poco::Logger::get("CameraDroneTransform");

通过 Poco 库获取一个日志对象 logger，名称为 "CameraDroneTransform"。

这种日志方式比 std::cout 更专业。可分级（信息、警告、错误等）。

void CameraDroneTransform::setCameraMount(const CameraMount& camera_mount) {
  camera_mount_ = camera_mount;
  camera_to_drone_rotation_ = computeCameraToDroneRotation();

  Poco::Logger& logger = Poco::Logger::get("CameraDroneTransform");
  logger.information("Camera mount configuration updated");
}


const CameraMount& camera_mount 是函数的参数。const 表示函数不会修改 camera_mount 这个对象。

CameraDroneTransform:: 表示这个函数属于 CameraDroneTransform 类（作用域限定符）。如果不写呢？

写上 CameraDroneTransform::，告诉编译器：“这个函数是 CameraDroneTransform 类的成员函数，不是一个普通的全局函数”。所以编译器会把这个函数和类的对象关联起来，并允许访问类的 成员变量 和 其他成员函数。

在类声明里直接写函数体也行的。

marker_pos_camera 是标志物在相机坐标系中的位置。

camera_to_drone_rotation_ 是一个旋转矩阵，将相机坐标系对齐到无人机机体坐标系。

git show <commit-hash>。

git show <commit-hash> 是用来查看 指定提交的详细信息的。

假设场景是 PX4 SITL 在本地运行，PX4 输出 MAVLink 数据到 localhost:14540（也就是本机的 14540）。

mavlink-router 在 0.0.0.0:14540 上监听，所以接收到数据。收到数据后，通过 -e 192.168.124.57:14540 转发给远程 PC。如果远程 PC 向路由器发送 MAVLink 消息（例如命令响应），mavlink-router 会把它路由回 PX4。

这个路由是双向的，PX4 就像和远程 PC 直接通信一样。

git log --oneline --all。

git show --name-only 4485a33。

git show 4485a33。

老板教了我vs code里面用control p来搜索文件哈哈。

cmake构建的时候，aarch64 debug和aarch64 release区别？

Release：主要用于发布和运行，程序运行更快，体积更小，但调试难度大。

Debug：主要用于开发和调试，能方便地用 gdb、lldb 查看变量、单步调试。

最简手工编译脚本示例?

假设源码都在 src/，入口是 main.cpp，生成的可执行文件放 build/。

老板写的build.sh，如果我只想在 RK3588 上跑，可以直接./build.sh aarch64 release。

git show <commit-hash>，如果输出内容太多，终端默认会用 分页器（通常是 less）显示。

按 回车 → 向下滚动一行。按 q → 退出分页器。按 空格 → 翻一屏。

ssh-copy-id orangepi@192.168.124.57 这样就可以省去下次输入密码了。老板教我的。

ssh orangepi@192.168.124.57 。

cd /home/orangepi/Documents/drone_toolbox .



ubuntu下deb包好像才是正统。

新部分，用rk3588测试。

mavlink-routerd -e 192.168.124.57:14540 0.0.0.0:14540。

监听本机 UDP 14540，把 MAVLink 转发到 192.168.124.57:14540。

export PX4_GZ_WORLD=baylands  。
make px4_sitl gz_x500_mono_cam_down 。


./aruco_landing_uvc_app 。


commander arm。


while (telemetry.armed()) {
    const auto pos = telemetry.position();
    logger.information("Status: armed, relative altitude " +
                       std::to_string(pos.relative_altitude_m) + " m");
    std::this_thread::sleep_for(2s);
  }







passthrough.queue_message(),不是“起飞”或“解锁”本身。


Action action(system);

action.arm();

MAVSDK 会自动发送一串 MAVLink。

#include <mavsdk/plugins/mavlink_passthrough/mavlink_passthrough.h>，直接发 MAVLink 原始包。

MAVLink = 一整套 XML 消息集合（几百个）。


最终产物：



libopencv_core.a
libopencv_imgproc.a
libopencv_imgcodecs.a
libopencv_calib3d.a
libopencv_aruco.a


我的应用
 ├── MAVSDK
 │    └── MAVLink headers
 │
 ├── OpenCV (aruco)
 │    └── Eigen
 │
 ├── POCO
 │
 ├── Cilantro (可选)
 │    └── Eigen
 │
 └── libc / libstdc++ / libatomic

 OpenCV 的输出只有一堆「静态库 + 头文件」。

 PX4 是一个状态机极其严格的飞控系统。

PX4 内部有： mavlink_receiver.cpp。

收到 MAVLink 消息，转换成 uORB，投喂到 PX4 内部模块（Commander / Navigator / Offboard）。

ubuntu24，我需要下载一个串口助手。然后通过cutecom来和rk3588开发板（香橙派 max）连接，串口通讯测试。

Orange Pi 端跑 echo 程序。Ubuntu 端 CuteCom 发指令。

orangepi5max:Downloads:% cd wiringOP.

orangepi5max:wiringOP:% gpio readall.

std::optional<T> 是 C++17 引入的一个模板类，用来表示“可能有值，也可能没有值”的情况。它就像一个包装器，内部可能包含一个类型为 T 的对象，也可能什么都没有。

has_value() 是 std::optional 的成员函数，用来判断 optional 里是否真的有值。

Drone toolbox这个代码，没有hardcord。这个要学习一下。

参数解析是运行时完成的。

./aruco_uvc_auto_app udp://:14540 /dev/video0 1280 1024 30 3.0。

./aruco_uvc_auto_app serial:///dev/ttyS1:115200 /dev/video0 1280 1024 30 3.0。

好像对于rk3588适用的代码。./build.sh aarch64 release 。



重新编译通常是因为程序内部的逻辑或常量在源码里硬编码了。float takeoff_alt_m = 3.0F;  // 如果后面没有解析 argv，就只能是 3.0。

重要!!!!可以融合第三方库到我的代码吗？？？？？？？？？


在 C++ 项目里，有两种文件需要区分：

你的源文件（.cpp/.cc/.cxx 等）。

第三方库的头文件（.h/.hpp 等）和库文件（.a/.so/.lib/.dll 等）。

我的代码中，蜂鸣器是作为 用户的状态反馈，告诉你无人机已经收到了控制指令。

C 通道 11 控制 ArUco 检测开关：

高电平（1950–2050） → 启用 ArUco检测 → 蜂鸣器 3声 提示开启。

低电平 (<1950) → 禁用 ArUco检测 → 蜂鸣器 1声 提示关闭。

等等，程序为什么是多线程的？？？

passthrough.subscribe_message(
    MAVLINK_MSG_ID_RC_CHANNELS,
    [&logger](const mavlink_message_t& message) {
        // 解析 RC 数据
        g_aruco_detection_enabled.store(true/false);
        g_buzzer_beep_count.store(1/3);
    });

main() 循环是主要线程。

video0的逻辑？？


写完新文件比如aruco_uvc_auto.cc后怎么编译成elf呢？？？





蜂鸣器操作涉及 MAVLink 发送指令，并且每次 beep 要间隔 150~200ms。

如果在主线程里直接阻塞处理，会拖慢图像处理循环（影响帧率）。

uvc的摄像头逻辑？？

和我想的逻辑不大一样，老板说我们的这个项目结构已经是工程最佳实践了。

后面可以加上三方包的软连接形式。

然后代码不是都融合到我们的代码里面就是好的，保持解耦也是最好的方法。

USB3 + WiFi 是 GPS 的天敌?????

USB 3.0 杂散噪声 ≈ 1.2 ~ 2.0 GHz（连续宽带）。

GPS 接收的是 –130 dBm 级别的信号。USB3/WiFi 发射的是 +10 ~ +20 dBm。

DSM RC Port (JST-ZH 1.5 mm) 是一种在飞控板（如 Pixhawk 系列）上用来接收遥控信号的小型连接器接口。


/data/open-src/PX4-Autopilot。



./gazebo_aruco_auto_takeoff_app \
    --topic /world/aruco/model/x500_mono_cam_down_0/link/camera_link/sensor/imager/image \ 
    --config config/aruco_landing_config.xml




<?xml version="1.0" encoding="UTF-8"?>
<sdf version='1.9'>
  <model name='arucotag'>
    <static>true</static>
    <pose>0 0 0.001 0 0 0</pose>
    <link name='base'>
      <visual name='base_visual'>
        <geometry>
          <plane>
            <normal>0 0 1</normal>
            <size>0.5 0.5</size>
          </plane>
        </geometry>
        <material>
          <diffuse>1 1 1 1</diffuse>
          <specular>0.4 0.4 0.4 1</specular>
          <pbr>
            <metal>
              <albedo_map>model://arucotag/marker.png</albedo_map>
            </metal>
          </pbr>
        </material>
      </visual>
    </link>
  </model>
</sdf>

OpenCV 是什么?

是一个用 C++ 写、对 Python 友好 的 计算机视觉 + 图像处理基础库。

光轴（Optical Axis）是镜头的几何对称轴，也是相机坐标系的 Z 轴方向。

fx = “方向偏一点，在屏幕上挪多少像素”。

镜头上写的，3.6 mm。这对相机厂商有用。但对算法没用。

“z=1”出现在归一化相机坐标系。

把一个三维点(X,Y,Z)，x=Z/X​,y=Z/Y​变成(x,y,1)。

z = 1 是“只保留方向，把距离丢掉”。

USB Camera
   ↓
OpenCV VideoCapture
   ↓
cv::cvtColor / resize
   ↓
cv::aruco::detectMarkers
   ↓
cv::aruco::estimatePoseSingleMarkers
   ↓
tvec / rvec
   ↓
坐标系变换（OpenCV → 机体 → NED）
   ↓
MAVLink / MAVSDK
   ↓
PX4 Position / Precision Land

OpenCV 的 rvec / tvec 是什么？？？？？？？？？？？？？

tvec代表目标原点在“相机坐标系”下的位置。

OpenCV 相机坐标系？？？？

X → 右
Y → 下
Z → 前（朝镜头外）

PX4 用的是什么坐标系？？？？？？？？？？？？？？

PX4 机体系（FRD）？？？？？？？？

X → 前 (Forward)
Y → 右 (Right)
Z → 下 (Down)


世界系（NED）？？？？？？？？？？？

X → North
Y → East
Z → Down

如果要通过 LandingTarget / VisionPositionEstimate 发 MAVLink，发 Body Frame 就够了。

PX4 用 NED（North-East-Down），不是“随便选的”，而是航空领域 + 传感器物理方向 + 数学稳定性 + 历史标准的最优解。

机器人 / ROS 常用 ENU。

航空器，重力加速度是正方向，这很正常。

用 NED，  a_z = +9.81。

机体系（Body Frame）就是“绑在飞机身上的坐标系”，飞机怎么转，它就怎么转。

相机是装在飞机上的，飞机又是在 世界里飞的。

在 PX4 / MAVLink / 航空领域，机体系是固定定义的。

机体系（Body frame，FRD）
Xb → 前 (Forward)
Yb → 右 (Right)
Zb → 下 (Down)

机体系 vs NED，唯一的区别是，是否随飞机旋转。


OpenCV 默认相机坐标系：

Xc → 右
Yc → 下
Zc → 前


Rc2b = 从相机坐标系到机体系的旋转矩阵。

如果你在相机坐标系里有一个点 p_camera，乘上 Rc2b，就得到同一点在机体系里的坐标 p_body。

地球固联坐标系。地球固联坐标系用于研究多旋翼飞行器相对于地面的运动状态，确定机体的空间位置坐标。它忽略地球曲率，即将地球表面假设成一张平面。通常以多旋翼起飞位置或者地心作为坐标原点 。 z轴垂直于地面向下。

机体坐标系与多旋翼固连，其原点 取在多旋翼的重心位置上。

地球固联坐标系怎么旋转到机体坐标系？

NED 坐标系就是地球固联坐标系哈哈哈哈哈哈。

在 PX4/飞控中，从 NED 到 Body 的旋转矩阵常用 Z-Y-X 顺序（Yaw-Pitch-Roll）欧拉角旋转来表示。

PX4 把飞控应用逻辑当作“用户程序”，NuttX 提供 RTOS 内核和硬件抽象。

PX4 在 NuttX 上创建多个任务：

  px4io 任务（I/O 接口）

  commander 任务（飞控模式、状态机）

  sensors 任务（传感器采样、滤波）

上层 PX4 算法无需关心具体 MCU 是 STM32F4、F7 还是 H7，只要调用 HAL 接口即可。

PX4每个模块对应一个独立任务（thread）。

make list_config_targets.

飞控刷固件方式有几种，取决于 MCU 和 Bootloader。

我发现 MicroAir 的H743飞控也是用的BMI270。



aruco_uvc_auto_app.cc是我最新写的代码，这个程序不是单线程，而是至少有 4 类执行上下文。

要注意MAVSDK 内部线程，蜂鸣器线程，信号处理线程（异步）和主线程。

只要一个变量被「不同线程读写」就必须 atomic / mutex。

static std::atomic<bool> g_running{true};

其实就是g_running = true;

std::atomic<bool> g_running{1}; 也行。

对了，C/C++ 语法上完全不需要 .h 文件。

编译器不区分.h文件。 C/C++ 不区分。

当然，cpp项目里面用.cc / .hh组合是最好的实践。Google C++ Style Guide 就是这么干的。

aruco_config.cc,这是一个用 Poco XML + OpenCV ArUco 的配置加载器。



std::ifstream file(file_path);
if (!file.good()) {
  logger.error("Configuration file not found");
  return std::nullopt;
}

这里表示，“这个 optional 现在 没有值”。等价于return {};

std::nullopt 不是“失败”，而是 “结果不存在，但这是被允许的情况”。

std::optional<Config>，成功 → return config;失败 → return std::nullopt;模板 + 类型参数。

px4(1.6.0),我的目录下面有GZ和GAZEBO-CLASIC,那当我输入make px4_sitl gz_x500_mono_cam_down的时候，运行的是哪一个?

用的是 GZ（Gazebo / gz-sim / Ignition），不是 GAZEBO-CLASSIC。

Gazebo Classic正在被逐步淘汰。

Gazebo (Ignition / gz-sim)是官方主推方向。

gz_x500_mono_cam_down说明，以 gz_ 开头，走的是 Tools/simulation/gz。

data/open-src/PX4-Autopilot/Tools/simulation/gz/models/arucotag这里面的ARUCOTAG.PNG可以怎么用？

现在老板打印的这个小的aruco实体码是用生成器生成的7*7_100。

确定是7*7吗？？

为什么Gazebo可以做到仿真？什么团队研发的？

Gazebo 内部集成了多个成熟的物理引擎（例如 ODE、Bullet、Simbody、DART），这些引擎负责计算 刚体动力学、重力、摩擦、碰撞反应等物理行为。因此它能真实再现实世界中的运动与力学效果，而不仅仅是位置变化。

Gazebo 最早由 美国南加州大学（University of Southern California）机器人实验室的 Dr. Andrew Howard 和他的学生 Nate Koenig 开发，起始于 2002 年秋，作为 Player 项目的一部分，用于进行多机器人环境的三维仿真。

AURCO码，DICT_6X6_250和DICT_6X6_1000区别？

cv::aruco::drawMarker(dict250, 42, 200, markerImg250);

200×200 是像素。

sdf = Simulation Description Format。

<specular>0.4 0.4 0.4 1</specular>，specular 表示 镜面反射颜色和强度。

x500 机体模型多大？

轴距（对角电机间距）：≈ 500 mm。

这和我们要用的四旋翼基本一样大。

在 PX4 v1.16 里，Gazebo（gz）target 的命名规则是：gz_<airframe>_<sensor/config>。

分析ulg数据，目前使用plotjuggler。juggler中文是玩弄戏法的人。

对了，PX4 的时间单位是 timestamp（μs）。

plotjuggler里面看到的参数就是qgc里面的vehicle configuration可调节的参数。

或者说，px4（飞控）把qgc里面的点击analyze出现的实时mavlink消息定期打包，最后汇总成ulg飞行数据，以供分析软件访问。

ULG文件 里面的timestamp是从416.69到441.68,这是什么意思？

说明PlotJuggler 的 PX4 插件自动识别了 timestamp，自动换算为秒了。也就是 这段 log 覆盖了大约 23.3 秒。

17：37有44秒。

17：41有72秒。 

17：42有1秒。

17：43的数据是69秒。

17：45的数据是117秒。

ULog 文件的“开始 = 飞控上电并启动 PX4”。

ULog 文件的“结束 = 飞控关闭 / 重启 / 掉电 / 人为停止日志”。

ULog“分段 / 分文件”的原因？

日志如何工作？？？？

默认行为，车辆解锁（arming） 时自动开始记录，解除锁定（disarming） 时停止。每次解锁都会生成一个新日志文件，存储在 SD 卡上。


17：41有72秒。515.36 [mc_pos_control] invalid setpoints。意思是位置控制器收到的目标（position / velocity / altitude）不合法。常见原因可能是，OFFBOARD 模式下 setpoint 中断（异常）或者GPS 突变导致 EKF 拒绝数据。然后触发 Blind Land，也就是盲目降落。然后522.42 [gps] GPS spoofing detected! (state: 2)，意思是PX4 的 GPS 驱动 主动判定 GPS 被欺骗 / 数据异常。导致EKF 拒绝 GPS，position setpoint 立刻 invalid，触发 failsafe。


https://docs.px4.io/main/en/msg_docs/。看消息定义。这是uORB Message Reference（uORB 消息参考）页面。值得注意的是，This list is auto-generated from the source code。

所以，好像，ulg里面的参数信息其实也来自uorb总线？？？？？？？？？？但只有一部分是。

https://docs.px4.io/v1.12/en/log/flight_log_analysis.html.

学会分析ulg，首先就是要初步知道ulg里面都有什么，然后哪些参数很重要必看。然后学习别人是怎么分析的，官网在线分析器的模板怎么分析的。

https://logs.px4.io/。

px4里面的local position坐标系就是我理解的地球固联坐标系。

home position就是起飞时的位置。

ulg分析，有些很重要的数据，gps altitude(msl)和barometer altitude。

vehicle local position setpoint也很重要，但是我没看懂。


https://rfly.buaa.edu.cn/course/ch/Lesson05V2.pdf 有空看一下。

ulg分析的时候一定要看油门！！！！还有roll和pitch。








17：41有72秒。 观察下来，有一段pitch上升尖峰，很像我的救机的操作。

515.36，[mc_pos_control] invalid setpoints。

515.36，[mc_pos_control] Failsafe: blind land。

522.42，[gps] GPS spoofing detected! (state: 2)。


最终分析，17：41那次就是我们出事故的那一次，地面站提示可以起飞后，我直接就起飞了，结果飞了几秒，就在我准备停到二维码上面的时候，发生了意外，gps信息被px4拒绝，导致模式被自动切换为altitude模式，为什么被切换成altitude模式？我翻了官方的资料，说是被程序切换成了不如position模式安全，但是比stablized模式更安全的altitude模式，但是意外又发生了，我们的高度也是不准的，导致飞控失控一样突然拉高高度，然后快要撞到墙面上。后面的行为就更危险了，飞机直接连roll和pitch都不听话，以特别快的速度向屋檐撞去。刮擦5s,没错，持续5s,还没坠毁。刮擦后稍微可控了，老板遥控降落到了地面。



px4四旋翼飞行过程中撞到了屋檐，然后持续刮擦4s,这个过程怎么在ulg中分析出来？

刮擦时的典型特征是实际姿态 ≠ 期望姿态。Roll / Pitch 持续偏差。控制器在“拼命拉回来，但拉不动”。

找到了，那一段过程我通过分析motors output图表找到的。

21号下午试机，表现不错，确实是usb3.0的干扰。加了屏蔽层之后不仅起飞前没有各式各样的警告了，飞行过程中的postion模式也挺稳。

值得注意的是，ulg分析中，px4似乎完全依赖gps的高度，根本没有采用板载气压计的高度。

打 航向 的杆子，或者 Offboard 要求转向，PX4 会生成一个 yaw setpoint（角度和角速度），然后从这个 setpoint 直接算出一个前馈项。这个前馈项 不等误差产生，直接送进控制器。

最终控制量 = yaw_ff_setpoint + yaw_PID_output。

更新 f071bf0..941fb72
Fast-forward
 .vscode/c_cpp_properties.json                           |  30 ++
 config/aruco_landing_gazebo.xml                         |  19 +-
 docs/src/reference/px4-offboard-control.md              | 637 ++++++++++++++++++++++++++++++++++++++++++
 include/drone_toolbox/aruco/aruco_landing_pipeline.hh   |  22 +-
 include/drone_toolbox/aruco/landing_target_publisher.hh |  61 +++-
 samples/CMakeLists.txt                                  |  28 ++
 samples/integrated/gazebo_aruco_landing_profile_app.cc  | 234 ++++++++++++++--
 samples/integrated/gazebo_aruco_vertical_move_app.cc    | 890 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 samples/integrated/gazebo_aruco_vertical_move_app.hh    | 132 +++++++++
 src/aruco/aruco_landing_pipeline.cc                     |  51 +++-
 src/aruco/landing_target_publisher.cc                   |  93 +++++--
 11 files changed, 2125 insertions(+), 72 deletions(-)
 create mode 100644 docs/src/reference/px4-offboard-control.md
 create mode 100644 samples/integrated/gazebo_aruco_vertical_move_app.cc
 create mode 100644 samples/integrated/gazebo_aruco_vertical_move_app.hh                                              ~25s 


这个drone toobox，我需要学一下他们的ai aid commands。


ArUcoLandingPipeline 做的事情是，图像 → 检测 ArUco → 位姿估计 → 相机系 → 机体系 → NED 系 →（可选）发给 PX4 做精准降落。

GzCamera模块理解。


logger.information("Marker NED=[" +
                   std::to_string(proc_result.marker_ned_absolute.x()) + ", " +
                   std::to_string(proc_result.marker_ned_absolute.y()) + ", " +
                   std::to_string(proc_result.marker_ned_absolute.z()) + "]");


marker_ned_absolute 是谁的原点？

通常是PX4 的 local_position NED 原点。这个好理解。

飞行流程（flow）中的单步执行状态机。

程序的逻辑本身就是状态机。

logger.information() 不是免费的。。。需要格式化字符串，做IO，和处理锁问题。

enum class FlowStepPhase {
    kIdle,
    kMoving,
    kCompleted
};


所以，case FlowStepPhase::kMoving:。

枚举或类的静态成员不需要对象，直接用类型名 + :: 来访问。

检测到 Marker 就让无人机向标记位置下降。


相机坐标系原点在镜头光心????????


X 轴：水平向右。

Y 轴：垂直向下（OpenCV 默认）。

solvePnP 根据 四个角点的像素位置 + 相机内参 计算 3D 位姿。








source /opt/ros/humble/setup.bash
ros2 run ros_gz_bridge parameter_bridge \
    "/world/precision_landing/model/x500_mono_cam_down_0/link/camera_link/sensor/camera/image@sensor_msgs/msg/Image[gz.msgs.Image" \
    "/world/precision_landing/model/x500_mono_cam_down_0/link/camera_link/sensor/camera/camera_info@sensor_msgs/msg/CameraInfo[gz.msgs.CameraInfo"



# 启动 Micro XRCE-DDS Agent（通过串口连接飞控）
ros2 launch circle_pilot_bringup micro_xrce_agent.launch.py \
    transport:=serial \
    serial_port:="/dev/ttyUSB0"  # 请根据您的串口设备调整此路径



## 飞控和RK3588串口通讯部分


飞控和RK3588串口通讯。

dmesg | grep tty。

插拔串口线，看哪个 tty 在变化。

对了，要自己选好开发板上你要的那组串口，要手动开启的。

波特率115200。

我插到了飞控的tele1。

mavlink status 。

UXRCE_DDS_CFG。

MAV_1_CONFIG。


ros2 topic list。

输出的是 PX4 通过 uXRCE-DDS 桥接过来的所有 uORB 话题。


v4l2-ctl --all。





























