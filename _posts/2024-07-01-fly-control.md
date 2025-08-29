---
title: "自制飞控（autopilot）pcb，运行px4,项目经验总结"

---

## 通用知识点汇总

1. 飞控芯片选用F4还是F7呢？F743（商用飞控最经典芯片型号）基本一直缺货。F7其他型号的也是经常缺货。
2. 飞控主板上给 gyro等等供电肯定要 一个转成3.3V的电压转换器。先选择 NCP 1117 3.3V regulator 吧。然后 IMU 选 MPU6000 就行。然后得有个 USB 接口，方便和电脑 通信。其他 电阻什么的，最好0402尺寸的，不然PCB板子会太大了。
3. MAVLink 就像无人机和上位机之间的“语言”，负责信息传递、指令下发和状态同步。掌握 MAVLink 可以让你独立开发上位机软件或无人机控制程序。
4. PX4 上位机就是地面控制软件（QGroundControl、MAVProxy）。
5. PX4 里面有一个 uORB（micro Object Request Broker） → 类似 事件系统 / 发布-订阅框架。

    - 各模块（传感器、控制器、任务管理）不直接调用对方，而是通过消息发布和订阅交互。

    - 这就是 事件驱动框架 + 环形缓冲区 在飞控中的使用案例。
    - 传感器模块 → 发布 IMU_DATA；估计器 (EKF) → 订阅 IMU_DATA，然后发布 STATE_ESTIMATE；控制器 → 订阅 STATE_ESTIMATE 和 RC_INPUT，输出控制量；电机驱动 → 订阅 CONTROL_OUTPUT，驱动 PWM。
6. 估计器 (EKF)是什么？

    EKF（扩展卡尔曼滤波，Extended Kalman Filter）是飞控系统里一个核心大脑的角色，主要用于状态估计。或者说，是一种用于多传感器融合的状态估计算法，帮助飞控系统实时得到可靠的位置、速度、姿态。它是无人机飞行稳定和导航的核心算法之一。
7. eFuse 是有实体的。

    - eFuse 的实体通常就是一个很短的 电阻，连接在芯片内部的逻辑电路里面。所以一旦写入就不可逆。
8. SBUS（Serial BUS） 是 Futaba 公司最早提出的一种 单线串行通信协议，主要用于 遥控器和接收机之间的数据传输。这个协议把多个通道（一般 16 个以上）的 PWM 信号 打包成串行数据帧，通过 单根信号线 传输给飞控或伺服。
9. 怎么在stm32fxxxxxxx的数据手册上找到，电源供电方面的说明？比如我看视频都要并联一个电容来着。

    我看了数据手册，但是没找到相关的说明，后面发现了这个要在硬件设计指南上面找。
10. usb接口的d+和d-连到stm32，要加电阻的。作用是阻抗匹配，减少反射和 EMI。
11. 4 pin JST-GH这是什么？

    - JST：一家日本连接器厂商（Japan Solderless Terminal）。
    - GH 系列：JST 出品的一类小型连接器系列。
    - 4 Pin JST-GH：指 GH 系列里的 4 芯（4 针脚）连接器。


---

## 项目经验

1. The following basic concepts should be kept in mind when designing drone cabling:

    - High-Power and signal cables should be separated as much as is practical.
    - Cable lengths should be the minimum needed to enable easy handling of wired components. The wire tension should be adequate to survive possible airframe deformations even in a crash landing (wires must not be the first thing to break).
    - Cable loops to reduce excess length should be avoided - use shorter lengths!
    - For digital signals you can decrease the baudrate to reduce radiated energy and increase the robustness of data transfer. This means that you may be able to use longer cables when high data rates are not needed.
2. GPS receivers and magnetometers are generally very sensitive to EMI. Therefore these should be mounted far away from RF sources (high-power cabling, ESCs, radio modems and its antenna). This may be insufficient if the cabling is badly designed.