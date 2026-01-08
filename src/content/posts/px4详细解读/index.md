---
title: PX4
published: 2025-12-13T15:02:26.394Z
---



PX4 的链接脚本？

PX4的的“真实内存分层图”。

![alt text](<Screenshot 2025-12-13 at 11.06.15 PM.png>)

PX4 极少 malloc。

这个我确实听说过，航空电子里面，特别是战斗机，代码不malloc。

MPC 接收 EKF2 的状态。

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

为什么 PX4 用 MPC 而不是 PID？

PX4 用的是“简化版 MPC”。

EKF2：眼睛 + 大脑，判断“我现在在哪”

MPC：高级驾驶员，规划“怎么踩油门和方向盘”

姿态控制器：执行层，控制电机

读取px4飞快的sd卡，

PX4 的“系统 / 固件 / OS”不在 SD 卡里，而是在飞控 MCU 的内部 Flash 里。

px4飞控装在机架的别的位置而不是中心，可以吗?

