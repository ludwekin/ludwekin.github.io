---
title: 复刻MK1.1四合一电调
published: 2026-01-23T17:16:58.659Z
---


## 关于MK1.1四合一电调硬件本身

控制半孔板采用四层板。

推荐采用3~6s航模电池供电，极限8s。

主控是AT32F421K8U7，雅特力科技ARM® Cortex®-M4微控制器，高达120MHz的CPU运算速度与内建数字信号处理器(DSP)，最高可支持64KB闪存存储器(Flash)及16KB随机存取存储器(SRAM)。



## 关于fd6288q

1. FD6288Q 介于 MCU 和 MOS 之间。

    输入是 MCU 的逻辑 PWM。输入脚是HINx / LINx。

    内建死区功能，可防止同一桥臂高边和低边 MOS 同时导通。

    FD6288Q可以快速充 MOS 栅极，快速放 MOS 栅极。

2. 什么是高边mos?

    高边 MOS = 接在“电源正极那一侧”的 MOS 管。只要它 在负载和电源正极之间，它就是高边。

    高边 MOS 需要特殊的驱动方式。最经典就是Bootstrap方式。

2. 可以拿它和常见芯片DRV8323作个对比。

## 大电容

It is normally reccomended to use a large capacitor onboard or nearby due to the rapidly switched currents. The required size of this capacitor will vary depending on the expected use of the esc (and hardware?). You should ensure you're using capacitors with low ESR (Equivalent series resistance) and high ripple currents.

## 关于am32

The esc works out where the motor is by measuring the back emf induced in the coils by the rotation of the motor. This is achieved by measuring the input voltages to the motor coils. These voltages can be up to (or in some cases exceed) the voltage supplied by the battery, so they cannot be read directly by the microcontroller. You need to use a voltage divider to reduce the voltage to a level the microcontroller can safely accept.

A lower reduction and high precision resistors will give more accurate results.

An am32 esc can output telemetry signals, which can be picked up and read by flight controllers or other devices. Telemetry, as well as related components such as voltage and current sensingcan be left out without impacting performance.





