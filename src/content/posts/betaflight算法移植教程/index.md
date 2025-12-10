---
title: 从Betaflight代码库来进行部分算法移植
published: 2025-12-11T06:14:13.149Z
category: 四旋翼
tags:
    - 飞控
    - betaflight
---

从Betaflight里面移植现成的算法是个不错的选择。

Fork原项目，git log回到2022年的提交记录，随便选一个，开始分析代码。2022年是个我认为的不错的切入点。

首先是Feedforward（前馈）代码。

这个一般我们在设计飞控的时候不会想出这个操作。

Feedforward = 飞控提前知道你“下一瞬间要转多快”，于是直接给电机加力，不等误差出现。

这个算法可以让四旋翼“跟手”极强。

实际操纵中，Betaflight 还是先把你的摇杆（0–1000）转成目标角速度（°/s）。setpoint_rate = function(rc_input)。

ff_raw = k * (setpoint_rate - previous_setpoint_rate)。

补充一下，Betaflight/穿越机遥控器刷新率？

商业遥控器，内部mcu扫描速度基本在 2kHz ～ 8kHz 之间。

算了换一个文章说吧。见“RemoteController硬件讲解.md”。


