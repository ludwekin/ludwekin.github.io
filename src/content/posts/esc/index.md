---
title: esc
published: 2025-12-26T07:34:57.650Z
tags:
    - 电子调速器
category: 四旋翼
---


## 关于四旋翼esc，一些疑问和解答

1. 怎么从qgc看我的px4的四旋翼的电调固件？

    没法。

    PX4 只能输出 PWM / DShot。就算开启了双向 DShot ，也没法看到电调的固件信息。

    对了，ULog 里 看不到「原始 PWM / DShot 波形或数值」，但 能看到「PX4 最终打算给电调的控制量（归一化）」。以及（如果支持）电调反馈状态。

2. ulg里面的actuator_controls_0和actuator_controls_1，咋有俩？？？？

    对多旋翼来说，真正起作用的是 actuator_controls_1 。

    内容是：

        control[0] = roll
        control[1] = pitch
        control[2] = yaw
        control[3] = thrust

## 工厂无标55A四合一，分析

观察型号1,这是一个四合一电调（好像来自工厂），55A，底板24个磨码mosfet,上板俩78l10加上4个未知芯片和4个639d0nu bddfc。丝印AM32。

## 微空科技AM32_55A四合一，分析

观察型号2（微空科技AM32_55A）,这也是一个四合一电调，不分上下板，使用了开窗加焊铜条工艺，上面有四个QF32MT F4AK8U7。

然后注意大电容的丝印，470 35V PH。

QF32MT F4AK8U7这是一个电调的核心主控芯片。将微控制器(AT32F421)和电机驱动芯片(ID6288)合封为一体了。
