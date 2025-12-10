---
title: Betaflight,代码逻辑详解
date: 2025-12-08T19:14:42+08:00
tags:
    - 飞控
image: preview_photos/betaflight.png
category: 四旋翼
slug: betaflight控制逻辑
published: 2025-12-10T13:06:51.016Z
---

为什么 Betaflight 不用双环？

用不了。穿越机抖动噪声太大，外环积分误差会很大。

Betaflight的Dynamic Notch 在做什么？

实时测 RPM ，根据转速计算电机噪声频率。然后把 notch 滤波器中心频率移动到当前噪声上。

Betaflight项目的Makefile作用。

Makefile 文件是用于自动化 Betaflight 固件构建过程的。它定义了如何编译源代码、链接目标文件以及生成最终的固件文件（.hex 或 .bin）。

src/main/sensors/gyro.c。

