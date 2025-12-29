---
title: esc
published: 2025-12-26T07:34:57.650Z
---


怎么从qgc看我的px4的四旋翼的电调固件？

没法。

PX4 只能输出 PWM / DShot。就算开启了双向 DShot ，也没法看到电调的固件信息。

对了，ULog 里 看不到「原始 PWM / DShot 波形或数值」，但 能看到「PX4 最终打算给电调的控制量（归一化）」。以及（如果支持）电调反馈状态。

ulg里面的actuator_controls_0和actuator_controls_1，咋有俩？？？？

对多旋翼来说，真正起作用的是 actuator_controls_1 。

内容是：

control[0] = roll
control[1] = pitch
control[2] = yaw
control[3] = thrust

