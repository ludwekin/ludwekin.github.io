---
title: PX4
published: 2025-12-13T15:02:26.394Z
---



1. PX4 的链接脚本？这是什么？

2. PX4的的“真实内存分层图”。

![alt text](<Screenshot 2025-12-13 at 11.06.15 PM.png>)

3. PX4 极少 malloc。

这个我确实听说过，航空电子里面，特别是战斗机，代码不malloc。

4. MPC 接收 EKF2 的状态。

再接收你给 PX4 的指令：

Offboard / Mission / Position mode：

目标位置

目标速度

目标加速度（可选）

然后计算出：

期望加速度 ax ay az

等价的：

期望推力

期望姿态（roll / pitch）

然后交给 姿态控制器。

5. 为什么 PX4 用 MPC 而不是 PID？

PX4 用的是“简化版 MPC”。

EKF2：眼睛 + 大脑，判断“我现在在哪”

MPC：高级驾驶员，规划“怎么踩油门和方向盘”

姿态控制器：执行层，控制电机

6. px4飞控的sd卡是什么作用？

PX4 的“系统 / 固件 / OS”不在 SD 卡里，而是在飞控 MCU 的内部 Flash 里。

7. px4飞控装在机架的别的位置而不是中心，可以吗?

8. pkill -f px4


## 关于仿真

PX4 uses the normal MAVLink module to connect to ground stations and external developer APIs like MAVSDK or ROS.

Ground stations listen to PX4's remote UDP port of 14550.

External developer APIs listen to PX4's remote UDP port of 14540.

Gazebo Harmonic binaries are provided for Ubuntu Jammy (22.04) .

mkdir -p ~/Desktop/PX4-Autopilot/Tools/simulation/gz/models/fractal_marker

cp "fractal marker module/fractal_marker_20cm.png" ~/Desktop/PX4-Autopilot/Tools/simulation/gz/models/fractal_marker/materials/textures/

cp "fractal marker module/fractal_marker_20cm.png" ~/Desktop/PX4-Autopilot/Tools/simulation/gz/models/

cp precision_landing_world.sdf ~/Desktop/PX4-Autopilot/Tools/simulation/gz/worlds/

cp precision_landing_world.sdf ~/Desktop/PX4-Autopilot/Tools/simulation/gz/worlds/

gz sim precision_landing_world.sdf

## uxrce

1. src/modules/uxrce_dds_client/dds_topics.yaml，这个文件是可以改动的。

    而且改动会很有用。

    可以限制话题发布速率，也可以直接禁用话题。

2. 有意思的是，机载电脑和px4链接时，似乎udp的带宽要大于串口。

    之前就遇到了，串口速率不够，然后px4给机载电脑发数据太多导致卡顿的现象。最后把px4的串口波特率改到了921600我记得。

3. bootloader.px4board这个文件干嘛的？

4. arm-none-eabi = 给 ARM 微控制器用的“裸机 GCC 交叉编译工具链”。



